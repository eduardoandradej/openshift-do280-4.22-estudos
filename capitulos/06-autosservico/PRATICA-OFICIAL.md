# Prática oficial — Capítulo 6

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`,
> **seguindo o enunciado do PDF na ordem**. Credenciais, nomes, imagens e arquivos estão
> no enunciado de cada atividade. Nenhum comando `lab` é executado por este repositório.
>
> Seguindo os passos do PDF na ordem, as atividades rodam sem erro. A seção **Armadilhas
> do ambiente** registra apenas inconsistências do texto e situações em que o erro
> aparece **se você sair da sequência**, com a forma de agir.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | Usuário principal | PDF |
|-------|------|---------------------|-------------|-------------------|-----|
| 6.2 | Guided Exercise | `selfservice-quotas` | Não | `developer`, depois `admin` | p. 256–262 |
| 6.4 | Guided Exercise | `selfservice-ranges` | Não | `admin` (pela console) | p. 267–277 |
| 6.6 | Guided Exercise | `selfservice-projtemplate` | Não | `admin` + usuários do grupo criado pelo `lab start` + `developer` | p. 286–293 |
| 6.7 | Lab | `selfservice-review` | **Sim** | `admin` | p. 294–301 |

Identificadores e avaliação verificados no PDF do Capítulo 6. O capítulo não tem quiz.

> ⛔ Os exercícios 6.6 e 6.7 alteram configurações **do cluster inteiro** (template de
> projeto e binding `self-provisioners`). Faça só o que o enunciado pede e sempre rode o
> `lab finish`, que devolve o cluster ao estado esperado pelas próximas atividades.

---

## 6.2 Guided Exercise: Project and Cluster Quotas

| Campo | Valor |
|-------|-------|
| Seção | 6.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `selfservice-quotas` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 256–262 |

**Competência praticada:** ver os requests de CPU de um projeto ocuparem o nó e deixarem
`Pending` um workload de outro projeto; como administrador, criar uma quota de requests de
CPU no projeto exagerado e observar seus dois efeitos (excesso e ausência de request);
comprovar que os demais projetos voltam a funcionar.

**Domine antes:** [6.1](6.1-quotas-de-projeto-e-cluster.md).

```bash
lab start selfservice-quotas
lab finish selfservice-quotas
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Quantos Pods ficam `Running` | Depende da CPU livre no nó; o PDF avisa que os valores podem variar | O que importa é haver Pods `Pending` com `Insufficient cpu`; use os números do seu cluster |
| Troca de usuário | A partir do login de administrador, os passos seguintes (inclusive a quota) são feitos como `admin` | Não volte para o `developer` no meio; siga o usuário de cada passo |
| Projeto `test` criado duas vezes | O enunciado cria, apaga e depois recria um projeto com o mesmo nome | Siga a ordem; o apagamento acontece vários passos antes da recriação |
| Nomes de Pods/ReplicaSets na saída impressa | Os hashes do PDF nem sempre batem entre si (ex.: evento de escala × nomes dos Pods) | Irrelevante; use os nomes do seu cluster |
| Repetir o exercício | O Deployment que pede 1 CPU continua ocupando o nó | `lab finish` antes de recomeçar |

**Verificação conceitual:** sem quota, um projeto barra outro por falta de CPU; com quota,
o excesso é barrado **no próprio projeto** (eventos `exceeded quota` e `must specify
requests.cpu`), e outro projeto consegue pedir CPU.

**Diagnóstico se algo falhar:** `oc get pod,deployment` → `oc events` → `oc describe node`
(Allocated resources) → `oc get quota -o yaml`.

---

## 6.4 Guided Exercise: Per-project Resource Constraints: Limit Ranges

| Campo | Valor |
|-------|-------|
| Seção | 6.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `selfservice-ranges` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 267–277 |

**Competência praticada:** pela console, criar um projeto e um Deployment sem recursos,
criar um limit range a partir do YAML de exemplo da console, comprovar que os Pods antigos
não mudam e que os novos recebem request e limit, comparar Deployment × Pod e analisar o
uso real na aba **Metrics**.

**Domine antes:** [6.3](6.3-limit-ranges.md).

```bash
lab start selfservice-ranges
lab finish selfservice-ranges
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Login na console | Há mais de um provedor de identidade na tela | Escolha o indicado no passo |
| Número de réplicas | O formulário da console cria o Deployment com **três** réplicas (o PDF conta com isso) | Não altere; o exercício olha qualquer um dos Pods |
| Recriar o Deployment | O nome é o mesmo; criar de novo sem apagar o anterior dá conflito | Apague antes, como manda o passo |
| Projeto selecionado | Criar o limit range ou o Deployment com outro projeto selecionado | Confira o seletor **Project** em cada tela |
| Valores de memória em **Metrics** | O uso real difere do PDF | Compare a ordem de grandeza (uso × request × limit) |
| Terminal em vez da console | O PDF permite, copiando o YAML do editor | Mantenha o mesmo nome, projeto e imagem |

**Verificação conceitual:** Pods anteriores ao limit range seguem sem `resources`; os
criados depois têm os padrões; o Deployment continua com `resources: {}`.

**Diagnóstico se algo falhar:** `oc get limitrange -n <projeto>` → `oc describe pod` →
eventos do ReplicaSet.

---

## 6.6 Guided Exercise: The Project Template and the Self-provisioner Role

| Campo | Valor |
|-------|-------|
| Seção | 6.6 |
| Tipo | Guided Exercise |
| Identificador `lab` | `selfservice-projtemplate` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 286–293 |

**Competência praticada:** restringir a criação de projetos a um grupo editando o binding
`self-provisioners`; comprovar com um usuário de fora e dois de dentro do grupo; testar um
limit range de máximo de memória em um namespace de rascunho; gerar o template de projeto,
dar `admin` ao grupo e incluir o limit range; publicar, ativar, aguardar o rollout e
validar com um projeto novo.

**Domine antes:** [6.5](6.5-template-de-projeto-e-self-provisioner.md) e
[6.3](6.3-limit-ranges.md).

```bash
lab start selfservice-projtemplate
lab finish selfservice-projtemplate
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Anotação `autoupdate` | O PDF diz que **não** é preciso torná-la `false` no exercício; a mudança vale até o API server reiniciar | Siga o PDF; em produção, a alteração permanente exige `autoupdate=false` (lição 6.5) |
| `oc edit` do binding | Indentação errada em `subjects` faz o editor reabrir com erro | Mantenha o alinhamento do conteúdo mostrado no passo |
| Troca de usuários | O exercício alterna `admin`, `developer` e os dois usuários do grupo; senhas no enunciado | Confira `oc whoami` antes de cada passo |
| Arquivos com placeholders | O limit range e o template são editados à mão | Compare com o conteúdo mostrado no passo antes de criar |
| Template: itens da lista | Copiar do `oc get ... -o yaml` traz `status`, `uid`, `resourceVersion` etc. e o namespace do rascunho | Deixe só o necessário e use `${PROJECT_NAME}` no namespace |
| Esperar o rollout | Criar o projeto de teste **antes** de o Pod novo do `openshift-apiserver` ficar `Running` | Aguarde no `watch` um Pod com **AGE** recente em `Running` |
| Projeto `test` criado duas vezes | Criado, apagado e recriado depois do template | Siga a ordem; o apagamento é bem anterior |
| Pular o `lab finish` | O template, a configuração global e o binding alterados afetam as próximas atividades | Sempre `lab finish selfservice-projtemplate` |

**Verificação conceitual:** `developer` recebe `You may not request a new project via this
API`; o segundo usuário do grupo acessa o projeto criado pelo primeiro (binding do template
para o grupo); o Deployment que pede 2 GiB não cria Pods (`maximum memory usage per
Container is 1Gi`).

**Diagnóstico se algo falhar:** `oc describe clusterrolebinding self-provisioners` →
`oc get template -n openshift-config` → `oc get projects.config.openshift.io cluster -o yaml`
→ `oc get pod -n openshift-apiserver` → `oc get rolebinding,limitrange -n test` →
`oc events`.

---

## 6.7 Lab: Enable Developer Self-service

| Campo | Valor |
|-------|-------|
| Seção | 6.7 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `selfservice-review` |
| Avaliação | **`lab grade selfservice-review`** |
| Referência | PDF p. 294–301 (enunciado p. 294–295; solução a partir da p. 296) |

**Competência praticada (resumo autoral):** projetar, em um namespace de rascunho, uma
quota de memória (requests e limits) e um limit range com padrão, mínimo e máximo de
memória; comprovar o efeito com um Deployment sem recursos escalado além da quota; levar
os dois objetos para o template de projeto (mantendo o `admin` do solicitante), ativá-lo
e validar com um projeto novo.

**Domine antes:** as três lições, os três guided exercises e a [revisão](REVISAO.md).

```bash
lab start selfservice-review
lab grade selfservice-review
lab finish selfservice-review
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Nome do namespace de rascunho | O PDF avisa: `lab grade` e limpeza só reconhecem o nome indicado | Use exatamente o nome do enunciado |
| Sobras do GE 6.6 | Sem o `lab finish` do 6.6, o namespace de rascunho e o template `project-request` já existem (`AlreadyExists`) | Rode o `lab finish` do 6.6 (e o `lab start` deste lab) antes |
| Ordem limit range → Deployment de teste | Criar o Deployment antes do limit range, com a quota já criada, deixa-o sem Pods (`must specify ...`) | Crie a quota e o limit range primeiro; se errou, recrie o Deployment |
| Interpretar o enunciado | "Não pedir mais que X no total" e "não usar mais que Y" são `requests.memory` e `limits.memory`; mínimo/máximo/padrões são do limit range | Releia a lição 6.1 e 6.3 antes de montar |
| Contagem de réplicas | O número que sobe resulta da conta quota ÷ padrões; não é erro ficar abaixo do pedido | Confira com `oc describe quota` |
| Template | Manter o binding `admin` do solicitante (não trocar por grupo, como no GE 6.6) | Parta do `create-bootstrap-project-template` |
| Esperar o rollout | Criar o projeto de validação antes do novo `openshift-apiserver` | Aguarde o Pod novo `Running` |

**Verificação conceitual:** o projeto de validação nasce com a quota e o limit range
corretos (`oc describe quota` e `oc describe limitrange`), e o solicitante é `admin`.

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc get template project-request
-n openshift-config -o yaml` → `oc get projects.config.openshift.io cluster -o yaml` →
`oc describe quota,limitrange -n <projeto de validação>`.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 6.5 Template de projeto e self-provisioner](6.5-template-de-projeto-e-self-provisioner.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
