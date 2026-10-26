# Prática oficial — Capítulo 7

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`,
> **seguindo o enunciado do PDF na ordem**. Credenciais, nomes, arquivos e versões estão no
> enunciado de cada atividade. Nenhum comando `lab` é executado por este repositório.
>
> Seguindo os passos do PDF na ordem, as atividades rodam sem erro. A seção **Armadilhas
> do ambiente** registra apenas inconsistências do texto e situações em que o erro
> aparece **se você sair da sequência**, com a forma de agir.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | Usuário principal | PDF |
|-------|------|---------------------|-------------|-------------------|-----|
| 7.2 | Quiz (associação) | — | — | — | p. 308–309 |
| 7.4 | Guided Exercise | `operators-web` | Não | `admin` (console + um passo na CLI) | p. 314–325 |
| 7.6 | Guided Exercise | `operators-cli` | Não | `admin` | p. 333–340 |
| 7.7 | Lab | `operators-review` | **Sim** | `admin` | p. 341–349 |

Identificadores e avaliação verificados no PDF do Capítulo 7.

> O ambiente do curso é **desconectado**: o OLM usa um único catálogo espelhado, só com os
> operators das atividades, e alguns operators têm um único canal. Nomes de catálogo e
> versões de CSV vêm desse ambiente; use os que o **seu** cluster mostrar.

---

## 7.2 Quiz: Kubernetes Operators and the Operator Lifecycle Manager

Quiz de associação entre termos e definições (PDF p. 308; solução na p. 309). Responda
antes de olhar a solução; este repositório não reproduz o gabarito. Base:
[lição 7.1](7.1-operators-e-olm.md).

---

## 7.4 Guided Exercise: Install Operators by Using the Web Console

| Campo | Valor |
|-------|-------|
| Seção | 7.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `operators-web` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 314–325 |

**Competência praticada:** instalar um operator pelo catálogo da console com as opções
padrão; ler condições, YAML e assinatura do CSV; (opcional) criar um CR, provocar uma
alteração no nó e ler o relatório produzido; distinguir o Deployment do operator do
workload criado pelo CR; desinstalar o operator e apagar o namespace criado na instalação.

**Domine antes:** [7.1](7.1-operators-e-olm.md) e [7.3](7.3-instalar-operators-pela-console.md).

```bash
lab start operators-web
lab finish operators-web
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Login na console | Há mais de um provedor de identidade na tela | Escolha o indicado no passo |
| **View Operator** antes da hora | O PDF avisa: o botão (e até erros) pode aparecer por instantes antes de o OLM terminar | Espere o botão ficar estável por alguns segundos |
| Projeto selecionado | Abas e listas vazias se outro projeto estiver selecionado | Confira o projeto indicado no passo |
| Campo do CR ausente no YAML | O PDF prevê que o campo pode não aparecer no editor | Alterne para **Form view** e volte para **YAML view**, como o PDF orienta |
| Resultado do CR demora | O status do nó e o relatório podem levar minutos; o PDF manda recriar o CR se nada surgir após o tempo indicado | Aguarde; recrie se preciso. O passo é opcional e não afeta o resultado do exercício |
| Alteração no nó | O exercício cria um arquivo no nó de propósito, para gerar o relatório | ⛔ Fora do exercício, não altere arquivos dos nós do cluster |
| Desinstalar sem apagar o namespace | O namespace criado pela instalação continua existindo | Apague-o no último passo, como o PDF manda |

**Verificação conceitual:** o CSV chega à fase `Succeeded`; o Deployment do operator é
diferente do DaemonSet criado a partir do CR; após a desinstalação, o namespace do
operator é removido.

**Diagnóstico se algo falhar:** **Installed Operators › Details › Conditions** →
`oc get subscription,installplan,csv -n <ns>` → `oc get deployment,pod -n <ns>` → eventos.

---

## 7.6 Guided Exercise: Install Operators with the CLI

| Campo | Valor |
|-------|-------|
| Seção | 7.6 |
| Tipo | Guided Exercise |
| Identificador `lab` | `operators-cli` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 333–340 |

**Competência praticada:** ler o `PackageManifest`; criar o namespace exigido pelo
operator (com labels), um OperatorGroup restrito a ele e uma Subscription com aprovação
manual; localizar e aprovar o install plan; confirmar a instalação; testar o operator com
um CR e ler o relatório que ele gera.

**Domine antes:** [7.5](7.5-instalar-operators-pela-cli.md).

```bash
lab start operators-cli
lab finish operators-cli
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Sobras do GE 7.4 | Segundo o PDF, o `lab start` remove o namespace e o operator se existirem | Basta rodar o `lab start` antes |
| Arquivos com placeholders | OperatorGroup e Subscription são editados à mão | Compare com o conteúdo do passo ou com os arquivos de solução citados pelo PDF |
| Ordem namespace → OperatorGroup → Subscription | Se inverter a ordem, a instalação passa a depender de um OperatorGroup que ainda não existe (o CSV indica `NoOperatorGroup` enquanto ele faltar) | Siga a ordem do PDF; se inverteu, crie o OperatorGroup e acompanhe o CSV |
| Nome do install plan | É gerado: o do PDF não vale no seu cluster, e o plano pode demorar a aparecer | Use o nome do **seu** `oc describe operator`; repita o comando se ainda não houver plano |
| Instalação não imediata | Depois da aprovação, a instalação leva um tempo | Repita a verificação, como o PDF orienta |
| Aviso de `DeploymentConfig` no `oc get all` | Mensagem de depreciação, não erro | Ignore |
| Primeiro CR sem resultado | O PDF avisa que o primeiro CR pode não funcionar e que a espera pode chegar a vários minutos | Aguarde; se nada surgir, apague e recrie o CR |
| Comandos com `--watch`/`watch` | Ficam rodando | `Ctrl+C` quando o objeto esperado aparecer |
| Versão do CSV | Pode diferir da impressa | Use a do seu cluster |
| Alteração no nó | Faz parte do exercício | ⛔ Fora do exercício, não altere arquivos dos nós |

**Verificação conceitual:** antes da aprovação, o recurso `Operator` mostra
`InstallPlanPending`; depois, `InstallSucceeded` e um Deployment pronto; o relatório do
operator registra o arquivo adicionado ao nó.

**Diagnóstico se algo falhar:** `oc describe operator <pacote>` → `oc get installplan -n
<ns>` → `oc get operatorgroup -n <ns>` → `oc get csv -n <ns>` → `oc get
deployment,pod -n <ns>` → `oc describe <CR>` (eventos).

---

## 7.7 Lab: Manage Kubernetes Operators

| Campo | Valor |
|-------|-------|
| Seção | 7.7 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `operators-review` |
| Avaliação | **`lab grade operators-review`** |
| Referência | PDF p. 341–349 (enunciado p. 341–342; solução a partir da p. 343) |

**Competência praticada (resumo autoral):** pela CLI, descobrir no `PackageManifest` os
dados de um operator de conformidade; criar o namespace sugerido, um OperatorGroup restrito
a ele e uma Subscription com aprovação automática; esperar o CSV e todos os Deployments do
operator; testar o operator com um CR de exemplo e esperar a conclusão da análise.

**Domine antes:** as três lições, os dois guided exercises e a [revisão](REVISAO.md).

```bash
lab start operators-review
lab grade operators-review
lab finish operators-review
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Nomes | Namespace, OperatorGroup e Subscription são conferidos pelo `lab grade` | Use exatamente os do enunciado |
| CR antes do operator pronto | Criar o CR antes de o CSV chegar a `Succeeded` falha, porque o tipo ainda não existe na API | Espere o CSV e os Deployments, como manda o enunciado |
| Deployments além do CSV | O CSV descreve um Deployment, mas o operator cria outros (o enunciado avisa) | Espere todos ficarem prontos |
| Versão do CSV | O PDF avisa que a versão pode mudar | Substitua pela do seu cluster nos comandos |
| Resultado da análise | `NON-COMPLIANT` é o resultado esperado: prova que o operator funciona | O que importa é a fase `DONE` |
| Tempo da análise | Pods extras aparecem e somem durante a análise; pode levar alguns minutos | Aguarde com `watch` |
| `jq` | Só formata a saída | Opcional |
| Repetir o lab | Namespace e objetos já existentes (`AlreadyExists`) | `lab finish` e `lab start` antes de recomeçar |

**Verificação conceitual:** CSV `Succeeded`; três Deployments prontos; o CR de exemplo gera
um resultado na fase `DONE`.

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc get operatorgroup,subscription
-n <ns>` → `oc get csv -n <ns>` → `oc get deployment -n <ns>` → `oc get <tipo do resultado>
-n <ns>`.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 7.5 Instalar operators pela CLI](7.5-instalar-operators-pela-cli.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
