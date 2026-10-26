# Prática oficial — Capítulo 5

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`,
> **seguindo o enunciado do PDF na ordem**. Credenciais, nomes, IPs e arquivos estão no
> enunciado de cada atividade. Nenhum comando `lab` é executado por este repositório.
>
> Seguindo os passos do PDF na ordem, as atividades rodam sem erro. A seção **Armadilhas
> do ambiente** registra apenas inconsistências do texto e situações em que o erro
> aparece **se você sair da sequência**, com a forma de agir.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | Usuário principal | PDF |
|-------|------|---------------------|-------------|-------------------|-----|
| 5.2 | Guided Exercise | `non-http-lb` | Não | `developer` | p. 207–212 |
| 5.4 | Guided Exercise | `non-http-multiple` | Não | `admin` | p. 225–233 |
| 5.5 | Lab | `non-http-review` | **Sim** | `developer` (+ `admin` para namespace, UDN e permissões) | p. 234–243 |

Identificadores e avaliação verificados no PDF do Capítulo 5. O capítulo não tem quiz.

> O ambiente tem **MetalLB** em camada 2 com um pool de pouquíssimos IPs (dois no GE 5.2 e
> um único no Lab 5.5, segundo o próprio enunciado). O `<pending>` que aparece quando o
> pool acaba faz parte do exercício.

---

## 5.2 Guided Exercise: Configure Load Balancer Services

| Campo | Valor |
|-------|-------|
| Seção | 5.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `non-http-lb` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 207–212 |

**Competência praticada:** implantar três instâncias de uma aplicação de vídeo (RTSP),
expô-las com Services `LoadBalancer`, testar com `nc` e com um player de vídeo, observar o
esgotamento do pool de IPs e liberar um endereço para outro serviço.

**Domine antes:** [5.1](5.1-servicos-load-balancer.md).

```bash
lab start non-http-lb
lab finish non-http-lb
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| IPs externos | O último octeto pode diferir do PDF | Use o IP que o **seu** `oc get services` mostrar |
| Terceiro serviço em `<pending>` | Esperado: o pool tem dois IPs | É o objetivo do passo; libere um IP como o enunciado manda |
| Player de vídeo (`vlc`) | Exige interface gráfica; não funciona por SSH (aviso do PDF) | Use um terminal **no** `workstation`; mensagens de erro do VLC no terminal podem ser ignoradas (PDF) |
| Repetir o exercício | Serviços `LoadBalancer` que sobrarem de outra tentativa ocupam o pool | `lab finish` antes de recomeçar |

**Verificação conceitual:** cada serviço com IP externo responde na porta do stream; o
terceiro só recebe IP depois que outro é removido.

**Diagnóstico se algo falhar:** `oc get svc` (EXTERNAL-IP) → `oc get endpoints` →
`nc -vz <ip> <porta>` → `oc get pods`.

---

## 5.4 Guided Exercise: Configure Additional Pod Networks

| Campo | Valor |
|-------|-------|
| Seção | 5.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `non-http-multiple` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 225–233 |

**Competência praticada:** criar um namespace preparado para UDN primária e uma UDN pela
console; implantar um banco de dados nela e comprovar o isolamento em relação à rede
padrão; criar uma NAD `host-device` com IP estático, anexá-la a outro banco e comprovar o
acesso a partir de uma máquina na rede física (e a falta de acesso a partir de outra).

**Domine antes:** [5.3](5.3-redes-adicionais-de-pods.md).

```bash
lab start non-http-multiple
lab finish non-http-multiple
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Namespace da UDN | O label de UDN primária só vale **na criação**. Se o namespace for criado de outra forma (ex.: `oc new-project`) antes do passo indicado, não dá para acrescentar o label | Crie o namespace exatamente pelo arquivo do passo; se errou, apague-o, aguarde a remoção e crie de novo |
| Ordem namespace → UDN → banco | Implantar antes de a UDN existir deixa o Pod sem subir (evento `invalid primary network state`) | Siga a ordem do PDF e confira as condições da UDN antes de implantar |
| Login na console | Há mais de um provedor de identidade na tela de login | Escolha o provedor indicado no passo |
| Nomes e IPs dos Pods | Diferem dos do PDF | Use os do seu cluster nos comandos `oc exec`/`psql` |
| Edição da NAD | Placeholders no arquivo; JSON inválido ou `name` diferente do `metadata.name` | O próprio PDF manda comparar com o arquivo de solução usando `diff` antes de criar |
| Banco com `host-device` | A interface física só pode estar em um Pod | Não escale esse Deployment; o arquivo já usa `strategy: Recreate` |
| Teste a partir do `workstation` | Termina por tempo: esperado (o `workstation` não está na rede física) | É o resultado pedido no último passo |

**Verificação conceitual:** a anotação `network-status` mostra a UDN como interface padrão
no primeiro banco e `net1` com o IP estático no segundo; a máquina da rede física acessa o
segundo banco; a rede padrão não alcança o primeiro.

**Diagnóstico se algo falhar:** `oc get ns <ns> --show-labels` → `oc get
userdefinednetwork -n <ns> -o yaml` (condições) → eventos do Pod → `oc get net-attach-def`
→ anotação `network-status`.

---

## 5.5 Lab: Expose Non-HTTP/SNI Applications

| Campo | Valor |
|-------|-------|
| Seção | 5.5 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `non-http-review` |
| Avaliação | **`lab grade non-http-review`** |
| Referência | PDF p. 234–243 (enunciado p. 234–235; solução a partir da p. 236) |

**Competência praticada (resumo autoral):** expor uma aplicação de vídeo com um Service
`LoadBalancer` e testá-la com o player; como administrador, criar um namespace com UDN
primária e delegar sua administração a um usuário comum; como esse usuário, implantar e
escalar uma aplicação na UDN, comprovar a comunicação entre as réplicas e o isolamento em
relação a uma aplicação na rede padrão.

**Domine antes:** as duas lições, os dois guided exercises e a [revisão](REVISAO.md).

```bash
lab start non-http-review
lab grade non-http-review
lab finish non-http-review
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Pool com um único IP | Criar mais de um Service `LoadBalancer` (ex.: repetir o `oc expose` com outro nome) deixa o segundo em `<pending>` | Mantenha um só; apague o excedente |
| Namespace com UDN | Mesmo cuidado do GE 5.4: label só na criação, e a UDN antes dos Pods | Siga a ordem: namespace com label → UDN → permissão → implantação |
| Permissão do usuário comum | Sem o RoleBinding, o usuário comum não entra no namespace criado pelo administrador | Conceda o papel pedido **antes** de trocar de usuário (Capítulo 3) |
| Teste de isolamento | O `curl` da rede padrão para a UDN termina por tempo | Esperado; use um tempo limite, como o PDF faz |
| Nomes | Projetos, namespace, faixa de IP e papel são conferidos pelo `lab grade` | Use exatamente os do enunciado |

**Verificação conceitual:** o stream abre no IP externo; as réplicas na UDN têm IPs da
faixa definida e se comunicam; a aplicação na rede padrão não alcança a UDN.

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc get svc -A | grep LoadBalancer`
→ `oc get userdefinednetwork -n <ns>` → `oc get rolebinding -n <ns> -o wide` → anotação
`network-status` dos Pods.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 5.3 Redes adicionais de Pods](5.3-redes-adicionais-de-pods.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
