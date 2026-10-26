# Prática oficial — Capítulo 4

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`,
> **seguindo o enunciado do PDF**. As credenciais, nomes, hostnames e arquivos estão no
> enunciado de cada atividade. Nenhum comando `lab` é executado por este repositório.
>
> **Seguindo os passos do PDF na ordem, copiando os comandos, as atividades rodam sem
> erro** (confirmado no ambiente pelo usuário deste repositório). A seção **Armadilhas do
> ambiente** de cada atividade registra apenas: (a) trechos em que o **texto impresso** do
> PDF é inconsistente e pode confundir quem lê sem executar; e (b) situações em que o erro
> aparece **se você sair da sequência** (repetir uma atividade, pular ou inverter passos,
> usar outro usuário ou outro diretório). Nada aqui substitui ou altera os passos oficiais.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | Usuário principal | PDF |
|-------|------|---------------------|-------------|-------------------|-----|
| 4.2 | Guided Exercise | `network-ingress` | Não | `developer` | p. 151–160 |
| 4.4 | Guided Exercise | `network-policy` | Não | `developer` (+ `admin` para rotular namespace) | p. 169–176 |
| 4.6 | Guided Exercise | `network-svccerts` | Não | `admin` | p. 181–185 |
| 4.7 | Lab | `network-review` | **Sim** | `admin` | p. 186–197 |

Identificadores e avaliação verificados no PDF do Capítulo 4. O capítulo não tem quiz.

### Regras gerais para não causar erro no ambiente

- Confira **quem** você é e **onde** está antes de cada bloco: `oc whoami` e `oc project`.
- Pod names, IPs e hashes do PDF são exemplos: use os do **seu** cluster.
- Ao editar arquivos fornecidos (placeholders `CHANGE_ME`), preserve a indentação e
  valide antes de criar: `oc create -f <arquivo> --dry-run=server` (não persiste nada).
  Exceção: `oc create route ...` ignora `--dry-run=server` (ver 4.1).
- Se você precisar **repetir** um passo de criação (`oc create -f`), ele falhará com
  `AlreadyExists`; para reaplicar um arquivo editado, use `oc apply -f`.
- Um `curl` bloqueado por NetworkPolicy não recebe recusa: espere alguns segundos e
  pressione **Ctrl+C** (o código de saída 130 é esperado).
- ⛔ Nunca apague o Secret `signing-key` de `openshift-service-ca` nem altere as
  NetworkPolicies dos namespaces `openshift-ingress*`/`openshift-dns*`: afetam o cluster
  inteiro.
- Se algo sair do controle, `lab finish <atividade>` seguido de `lab start <atividade>`
  recomeça do zero (aguarde a remoção do projeto, que pode levar alguns minutos).

---

## 4.2 Guided Exercise: Protect External Traffic with TLS

| Campo | Valor |
|-------|-------|
| Seção | 4.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `network-ingress` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 151–160 |

**Competência praticada:** expor uma aplicação por Route HTTP e observar o tráfego em
claro; criar uma Route edge com o certificado da plataforma e comprovar que o trecho
interno continua em claro; gerar um certificado assinado pela CA de treinamento, montá-lo
em uma nova versão da aplicação e expô-la com Route passthrough, comprovando a
criptografia de ponta a ponta.

**Domine antes:** [4.1](4.1-tls-no-trafego-externo.md).

```bash
lab start network-ingress
lab finish network-ingress
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------|----------------------|
| Criação do projeto (passo 1) | Texto: a introdução diz que o `lab start` cria o projeto, e o passo 1 manda criá-lo; seguindo o PDF, o passo 1 funciona. O `already exists` só apareceria se o projeto sobrasse de uma execução anterior (ex.: repetir sem `lab finish` concluído) | Se aparecer, `oc project <projeto>` e siga; para repetir do zero, `lab finish` e depois `lab start` |
| Captura com `tcpdump` | Exige `sudo`; o nome da interface pode diferir do mostrado | Confirme a interface do IP do `workstation` com `ip addr` antes; encerre com Ctrl+C |
| Assunto do CSR (`-subj`) | Quebrar a linha dentro das aspas gera um assunto errado | Digite o `-subj` em **uma** linha (aviso do próprio PDF) ou responda ao prompt interativo |
| Diretório ao criar o Secret TLS | Texto: o prompt impresso nesse passo mostra outro diretório, mas, na sequência do PDF, você ainda está no diretório dos certificados e o comando funciona. Só falha se você mudar de diretório antes | Execute na ordem; se mudou de diretório, use o caminho relativo dos certificados |
| Nome da Route passthrough | Usa o mesmo nome da Route edge; o PDF manda apagar a edge antes. Só falha (`AlreadyExists`) se esse passo for pulado | Siga a ordem do PDF |
| Saída de `oc status` | A tag de imagem exibida no PDF difere da tag citada no texto | Diferença do material; confie no arquivo da atividade |
| Aviso do Firefox e emissor da Route edge | "Emissor não reconhecido pela Mozilla"; o texto do PDF atribui o certificado à CA do Ingress Operator, mas o `issuer` mostrado é a CA da sala de aula | Esperado: o certificado padrão do ingress do laboratório foi substituído pela CA da sala de aula (ver 4.1) |
| Curl dentro do Pod de debug | O Pod não tem a CA de treinamento | O `-k` usado ali é intencional; fora do cluster use `--cacert` |

**Verificação conceitual:** em HTTP você lê o conteúdo no `tcpdump`; na Route edge o
`curl -v` mostra o certificado curinga da plataforma e o Service responde em HTTP por
dentro; na passthrough o emissor é a CA de treinamento e o Service redireciona HTTP para
HTTPS.

**Diagnóstico se algo falhar:** `oc get pods` (a versão nova está pronta?) →
`oc set volumes deployment/<d>` (Secret montado?) → `oc get route` (tipo e porta) →
`curl -v --cacert <ca>`.

---

## 4.4 Guided Exercise: Configure Network Policies

| Campo | Valor |
|-------|-------|
| Seção | 4.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `network-policy` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 169–176 |

**Competência praticada:** testar a conectividade entre Pods do mesmo projeto e de
outro projeto; aplicar um default-deny; liberar uma origem específica (Pod em namespace
rotulado) para uma porta; comprovar o que continua bloqueado; e criar a política para
os routers.

**Domine antes:** [4.3](4.3-network-policies.md) (especialmente "E" × "OU" e o caso
do nó único).

```bash
lab start network-policy
lab finish network-policy
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------|----------------------|
| Nomes e IPs dos Pods | Diferem dos do PDF | Use a saída do script de informações fornecido pela atividade (segundo terminal) |
| Projeto atual ao criar políticas | No meio do exercício o projeto atual passa a ser o **outro** namespace | Crie as políticas com `-n <projeto-de-destino>`, como o PDF faz; sem `-n`, a política vai para o projeto errado e os testes não batem |
| Rotular o namespace | `developer` não tem permissão sobre Namespaces (`Forbidden`) | Faça esse passo como `admin`, como indicado, e **volte** ao `developer` depois (`oc whoami`) |
| Testes que devem falhar | O `curl` fica sem resposta | Aguarde alguns segundos e Ctrl+C (exit 130 esperado); opcionalmente `curl -m 5` |
| Route responde mesmo com default-deny | Cluster de nó único, routers na rede do host | Comportamento esperado e explicado no PDF; crie a política dos routers mesmo assim |
| Edição dos arquivos de política | Indentação errada muda o significado (E × OU) | Compare com o que o enunciado pede; confira com `oc describe networkpolicy` |

**Verificação conceitual:** antes do default-deny tudo responde; depois, nada responde
entre Pods; com a política específica, só a origem e a porta liberadas respondem.

**Diagnóstico se algo falhar:** `oc get networkpolicy -n <ns>` → `oc describe
networkpolicy <np> -n <ns>` → `oc get pods --show-labels -n <ns>` → `oc get ns
<origem> --show-labels`.

---

## 4.6 Guided Exercise: Protect Internal Traffic with TLS

| Campo | Valor |
|-------|-------|
| Seção | 4.6 |
| Tipo | Guided Exercise |
| Identificador `lab` | `network-svccerts` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 181–185 |

**Competência praticada:** gerar um certificado de serviço com o service-ca, montá-lo no
servidor com um patch, comprovar o erro de verificação em um cliente sem a CA, injetar a
CA em um ConfigMap e montá-la em um cliente, e comprovar a verificação bem-sucedida.

**Domine antes:** [4.5](4.5-tls-no-trafego-interno.md).

```bash
lab start network-svccerts
lab finish network-svccerts
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------|----------------------|
| Projeto e recursos iniciais | O `lab start` já cria o projeto, o Deployment do servidor e um Pod de teste sem CA; o PDF apenas entra no projeto | Não é preciso criar nada além do que os passos pedem |
| Patch do servidor | O arquivo fornecido tem placeholders; o nome do container precisa ser o real | Preencha conforme o enunciado; um nome de container errado faz o patch tentar **adicionar** um container sem imagem (recusado) |
| Ordem anotação → montagem | Montar um Secret que ainda não existe deixa o Pod pendente | Anote o Service e confirme o Secret antes de aplicar o patch |
| ConfigMap do bundle | O service-ca **substitui todo** o conteúdo do ConfigMap anotado | Use o ConfigMap vazio e dedicado que o enunciado pede |
| "Pod cliente" | O arquivo do cliente cria um **Deployment** | Use `oc exec deploy/<nome> -- ...` |
| Arquivos de solução | O PDF informa que há versões completas em `~/DO280/solutions/` | Use para **comparar** se travar, depois de tentar |
| Rotação da CA | A seção 4.5 mostra como rotacionar a CA do service-ca | ⛔ **Não** faça no ambiente: afeta o cluster inteiro |

**Verificação conceitual:** o Pod sem CA mostra erro de cadeia autoassinada; o cliente com
o bundle obtém `Verify return code: 0 (ok)` e o conteúdo via HTTPS.

**Diagnóstico se algo falhar:** `oc describe svc <s>` (anotação) → `oc get secret` →
`oc describe pod` do servidor (volume, eventos) → `oc get cm <ca> -o yaml`
(`service-ca.crt` presente?).

---

## 4.7 Lab: Configure Network Security

| Campo | Valor |
|-------|-------|
| Seção | 4.7 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `network-review` |
| Avaliação | **`lab grade network-review`** |
| Referência | PDF p. 186–197 (enunciado p. 186–188; solução a partir da p. 189) |

**Competência praticada (resumo autoral):** em uma API com dois microsserviços, cifrar o
tráfego interno com certificado de serviço e CA injetada, expor o serviço de entrada com
certificado corporativo por Route passthrough, e restringir a entrada de cada
microsserviço com NetworkPolicies (um aceitando só o outro microsserviço; o outro, só os
routers).

**Domine antes:** as três lições, os três guided exercises e a [revisão](REVISAO.md).

```bash
lab start network-review
lab grade network-review
lab finish network-review
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------|----------------------|
| Projeto | O `lab start` já cria o projeto e implanta os dois microsserviços | Só entre no projeto e no diretório da atividade |
| Estado intermediário | Depois de habilitar TLS no microsserviço interno e **antes** de o outro passar a usar HTTPS, a API devolve o valor de falha descrito no enunciado | Esperado durante a transição; siga a ordem dos passos |
| TLS × probes | Habilitar TLS sem mudar as probes para HTTPS deixa os Pods novos `0/1` e o rollout parado | Faça as duas mudanças **no mesmo** `oc apply`, como o enunciado descreve |
| Porta do Service | O Service interno passa a expor a porta HTTPS padrão, apontando para a porta do container | Confira `port` e `targetPort` e os endpoints |
| Reaplicar arquivos | `oc create -f` em objeto existente falha | Para reaplicar, `oc apply -f` |
| Secret TLS do certificado corporativo | Caminhos relativos ao diretório da atividade | Execute a partir do diretório indicado |
| Ordem Secret → Deployment | Aplicar o Deployment que monta o Secret TLS **antes** de criar o Secret deixa o Pod novo pendente (`secret ... not found`) | Crie o Secret primeiro, como na ordem do enunciado; se já aplicou, crie o Secret e aguarde o Pod sair do estado pendente |
| Route | Hostname e porta precisam coincidir com o certificado e o Service | Use exatamente os do enunciado; teste com `curl --cacert` |
| NetworkPolicies | Aplicadas no projeto atual | Confira `oc project` antes; os nomes são conferidos pelo `lab grade` |
| Política dos routers | No nó único, a Route funciona mesmo sem ela | Crie-a mesmo assim: o `lab grade` verifica |

**Verificação conceitual:** as duas URLs do enunciado respondem sem erro; o
microsserviço interno só aceita o de entrada; o de entrada só aceita os routers.

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc get pods` (algum `0/1`?) →
`oc describe pod` (probes, volumes) → `oc exec deploy/<entrada> -- curl -s https://<svc>`
→ `oc get netpol` e `oc describe netpol`.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 4.5 TLS no tráfego interno](4.5-tls-no-trafego-interno.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
