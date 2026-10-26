# Prática oficial — Capítulo 8

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
| 8.2 | Guided Exercise | `appsec-scc` | Não | `developer`, com `admin` para SCC e RBAC | p. 356–361 |
| 8.4 | Guided Exercise | `appsec-api` | Não | `admin`, depois `developer` | p. 367–371 |
| 8.6 | Guided Exercise | `appsec-prune` | Não | `admin` | p. 378–384 |
| 8.7 | Lab | `appsec-review` | **Sim** | `developer`, com `admin` quando necessário | p. 385–392 |

Identificadores e avaliação verificados no PDF do Capítulo 8. O capítulo não tem quiz.

> Regra que vale para todo o capítulo: **`-z <sa>` usa o projeto atual.** Depois de cada
> troca de usuário com `oc login`, confira o projeto (`oc project`) antes de comandos com
> `-z`.

---

## 8.2 Guided Exercise: Control Application Permissions with Security Context Constraints

| Campo | Valor |
|-------|-------|
| Seção | 8.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `appsec-scc` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 356–361 |

**Competência praticada:** implantar uma imagem que exige root e ver a falha; descobrir a
SCC que resolve; conceder a SCC a uma service account pelo atalho de cluster e associá-la
ao Deployment; repetir em outro projeto concedendo a SCC só ao projeto por Role e
RoleBinding; confirmar a SCC aplicada nos Pods.

**Domine antes:** [8.1](8.1-security-context-constraints.md).

```bash
lab start appsec-scc
lab finish appsec-scc
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Falha inicial do Pod | Esperada: é o ponto de partida. Pode demorar e aparecer como `Error` ou `CrashLoopBackOff` (o PDF avisa) | Aguarde e siga para os logs |
| Troca de usuário | Conceder SCC e criar Role/RoleBinding de SCC exigem `admin`; o resto é feito como `developer` | Siga o usuário de cada passo; `oc whoami` em caso de dúvida |
| `-z` após trocar de usuário | A SA é procurada no projeto atual | Confira `oc project` antes do `add-scc-to-user` |
| Nome do Pod em variável | O nome muda a cada redeploy | Capture de novo quando o PDF manda (o seletor do passo filtra pelo Deployment) |
| Rota com hostname | Pede o hostname indicado no passo | Use o do enunciado |

**Verificação conceitual:** antes, o Pod usa `restricted-v2` e falha; depois de associar a
SA, `openshift.io/scc: anyuid` nos dois projetos — em um por ClusterRoleBinding, no outro
só por RoleBinding.

**Diagnóstico se algo falhar:** `oc logs` → `oc adm policy scc-subject-review` →
`oc get deploy <d> -o jsonpath='{..serviceAccountName}'` → `oc get rolebinding -n <ns>` →
`oc describe pod | grep scc`.

---

## 8.4 Guided Exercise: Allow Application Access to Kubernetes APIs

| Campo | Valor |
|-------|-------|
| Seção | 8.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `appsec-api` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 367–371 |

**Competência praticada:** criar uma service account para um controller e referenciá-la no
manifesto do Deployment; em outro projeto, conceder a essa SA um papel por RoleBinding com
o nome de usuário completo da SA; alterar um ConfigMap e ver o controller reiniciar a
aplicação, que passa a mostrar a nova configuração.

**Domine antes:** [8.3](8.3-acesso-de-aplicacoes-a-api.md).

```bash
lab start appsec-api
lab finish appsec-api
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Nome da service account | O texto do passo 2 cita a SA sem o sufixo; o comando e o manifesto usam o nome com sufixo | Use o nome do **comando** em todos os lugares |
| Ordem SA → Deployment | Aplicar o Deployment antes de criar a SA deixa o ReplicaSet sem criar Pods (evento sobre a SA inexistente) | Crie a SA antes, como o PDF; se inverteu, crie a SA e aguarde |
| Diretório de trabalho | Os comandos usam caminhos relativos ao diretório do lab | Rode-os de dentro do diretório indicado; volte ao home no fim |
| Binding no projeto certo | O RoleBinding fica no projeto da **aplicação**, com a SA de outro projeto como sujeito | Use o nome completo da SA, como o passo mostra |
| Reação do controller | A nova configuração demora alguns segundos a aparecer | Mantenha o `watch` até a resposta mudar; `Ctrl+C` |

**Verificação conceitual:** o RoleBinding no projeto da aplicação referencia a SA do
projeto do controller; ao mudar o ConfigMap, a aplicação é reiniciada e o endpoint mostra a
descrição nova.

**Diagnóstico se algo falhar:** `oc get pods -n <ns-do-controller>` → `oc logs
deploy/<controller> -n <ns-do-controller>` (procure `Forbidden`) → `oc get rolebinding -n
<ns-da-app> -o wide` → `oc auth can-i update deployments -n <ns-da-app> --as
system:serviceaccount:<ns>:<sa>`.

---

## 8.6 Guided Exercise: Cluster and Node Maintenance with Kubernetes Cron Jobs

| Campo | Valor |
|-------|-------|
| Seção | 8.6 |
| Tipo | Guided Exercise |
| Identificador `lab` | `appsec-prune` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 378–384 |

**Competência praticada:** listar e remover imagens não usadas do nó com `oc debug` e
`crictl`; montar um script em ConfigMap e um CronJob que o executa; ver o primeiro Job
falhar por permissão; criar uma SA com papel de cluster e SCC privilegiada, referenciá-la no
CronJob e ver a limpeza funcionar.

**Domine antes:** [8.5](8.5-manutencao-com-cron-jobs.md), [8.1](8.1-security-context-constraints.md) e [8.3](8.3-acesso-de-aplicacoes-a-api.md).

```bash
lab start appsec-prune
lab finish appsec-prune
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Mensagens `RemoveImage` na limpeza manual | Imagens em uso não podem ser removidas; o PDF diz que esses erros podem ser ignorados | Ignore |
| Imagem do `oc` removida na limpeza manual | A limpeza também apaga a imagem que o CronJob usa; ela é baixada de novo na execução | Esperado |
| Primeiro Job `Failed` | **Esperado**: mostra a falta de permissão da SA `default` | Leia o log e siga o passo de permissões |
| Arquivos com placeholders | ConfigMap e CronJob são editados à mão; aninhamento errado gera erro no `oc apply` | Compare com os arquivos de solução citados pelo PDF |
| Aspas no `schedule` | Expressão começando com `*` sem aspas é YAML inválido | Mantenha as aspas, como no arquivo |
| Espera pelo agendamento | O CronJob roda a cada poucos minutos | Aguarde no `watch`; `Ctrl+C` quando o Job concluir |
| Nomes de Job/Pod | Gerados; diferem do PDF | Use os do seu `oc get` |
| Permissões da SA | `cluster-admin` + `privileged` só para o exercício | ⛔ Não reproduza fora do curso; não rode limpeza de nós fora dos exercícios |
| Diretório de trabalho | Caminhos relativos ao diretório do lab | Volte ao home no fim, como o PDF |

**Verificação conceitual:** sem SA, o Job falha com `Forbidden` para a SA `default`; com a
SA certa, o Job conclui e o log mostra as imagens removidas.

**Diagnóstico se algo falhar:** `oc get cronjobs,jobs,pods` → `oc logs pod/<pod>` →
`oc get cronjob <cj> -o jsonpath='{..serviceAccountName}'` → `oc auth can-i list nodes
--as system:serviceaccount:<ns>:<sa>` → `oc get clusterrolebinding | grep <sa>`.

---

## 8.7 Lab: Manage Application Security

| Campo | Valor |
|-------|-------|
| Seção | 8.7 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `appsec-review` |
| Avaliação | **`lab grade appsec-review`** |
| Referência | PDF p. 385–392 (enunciado p. 385–386; solução a partir da p. 387) |

**Competência praticada (resumo autoral):** fazer rodar uma aplicação legada que precisa
de root (descobrir a SCC, criar SA, conceder, associar e testar de dentro do Pod); preparar
uma aplicação de limpeza de projetos com SA própria e um papel de cluster fornecido;
transformar o manifesto de Pod dela em um CronJob com o agendamento pedido; (opcional) gerar
projetos de teste e conferir no log que foram removidos.

**Domine antes:** as três lições, os três guided exercises e a [revisão](REVISAO.md).

```bash
lab start appsec-review
lab grade appsec-review
lab finish appsec-review
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Nomes | O `lab grade` avalia o resultado pelos nomes do enunciado (projeto, service accounts, CronJob) | Use exatamente os do enunciado |
| Quem faz o quê | Conceder SCC, criar o papel de cluster e o ClusterRoleBinding exigem `admin` | Troque de usuário quando preciso e confira `oc project` antes do `-z` |
| Teste da aplicação legada antes da SA | Falha esperada até associar a SA com a SCC | Siga a ordem |
| Montar o `jobTemplate` | Copiar o `spec` do Pod para o nível certo do CronJob; o PDF avisa que o nome longo da imagem deve ficar em **uma linha** | Confira o aninhamento e compare com a solução citada pelo PDF |
| Aspas no `schedule` | Expressão começando com `*` | Mantenha entre aspas |
| Log da limpeza | O PDF avisa: a remoção pode estar no log do Pod **anterior** | Leia os logs dos últimos Pods concluídos |
| Projetos de teste | Só os projetos com o label do enunciado são removidos; o script cria esses projetos | Etapa opcional; aguarde a próxima execução |

**Verificação conceitual:** o Pod da aplicação legada roda com a SCC que permite root e responde de
dentro do container; o CronJob roda a cada minuto com a SA própria e remove os projetos de
teste vencidos.

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc describe pod | grep scc` →
`oc get deploy,cronjob -o yaml | grep serviceAccountName` → `oc auth can-i delete
namespaces --as system:serviceaccount:<ns>:<sa>` → `oc logs` dos Pods do CronJob.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 8.5 Manutenção com cron jobs](8.5-manutencao-com-cron-jobs.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
