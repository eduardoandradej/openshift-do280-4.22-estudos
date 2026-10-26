# Prática oficial — Capítulo 10

> Ponte para as atividades do curso. **Não** reproduz enunciados, especificações,
> valores ou soluções. Execute tudo no ambiente oficial, no `workstation`, como
> `student`, **seguindo a especificação do PDF**. Credenciais, nomes, imagens, valores e
> arquivos estão no enunciado de cada lab. Nenhum comando `lab` é executado por este
> repositório.
>
> Os três labs são de **especificação**: dizem o resultado esperado, não os passos, e
> a solução da apostila é resumida. A seção **Armadilhas do ambiente** registra
> inconsistências do texto e situações que causam erro se a ordem lógica não for
> respeitada.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | Tempo de referência | PDF |
|-------|------|---------------------|-------------|---------------------|-----|
| 10.2 | Lab | `compreview-review` | **Sim** | 60 min | p. 437–445 (enunciado p. 437–440; solução a partir da p. 441) |
| 10.3 | Lab | `compreview-apps` | **Sim** | 45 min | p. 446–458 (enunciado p. 446–449; solução a partir da p. 450) |
| 10.4 | Lab | `compreview-package` | **Sim** | 45 min | p. 459–467 (enunciado p. 459–462; solução a partir da p. 463) |

Identificadores, avaliação e tempos verificados no PDF do Capítulo 10.

> **Diretório dos arquivos.** Os três labs imprimem os caminhos como `~/DO0019L/labs/...`
> e `~/DO0019L/solutions/...`, enquanto todos os outros capítulos usam `~/DO280/...`
> (workspace do curso). Antes de começar, confira com `ls ~` qual diretório o `lab start`
> criou e use esse.

> **Ordem dos labs.** O `lab start` do 10.3 restaura a configuração de template de projeto
> do 10.2. Faça os labs na ordem e rode sempre o `lab finish` do anterior.

---

## 10.2 Lab: Set up Cluster Self-service

| Campo | Valor |
|-------|-------|
| Seção | 10.2 |
| Tipo | Lab de revisão |
| Identificador `lab` | `compreview-review` |
| Avaliação | **`lab grade compreview-review`** |
| Referência | PDF p. 437–445 |

**Competência praticada (resumo autoral):** criar grupos e dar a eles papéis de cluster
(inclusive um papel próprio para gerenciar grupos); restringir a criação de projetos a
grupos específicos de forma **permanente**; desenhar e testar em um namespace de rascunho
uma quota, um limit range e uma network policy; montar o template de projeto com binding,
label do projeto e esses recursos; validar criando um projeto com o usuário certo e
delegando acesso a um grupo de participantes.

**Domine antes:** [3.1](../03-autenticacao-autorizacao/3.1-provedor-de-identidade-htpasswd.md) ·
[3.3](../03-autenticacao-autorizacao/3.3-permissoes-com-rbac.md) ·
[4.3](../04-seguranca-de-rede/4.3-network-policies.md) ·
[6.1](../06-autosservico/6.1-quotas-de-projeto-e-cluster.md) ·
[6.3](../06-autosservico/6.3-limit-ranges.md) ·
[6.5](../06-autosservico/6.5-template-de-projeto-e-self-provisioner.md).

```bash
lab start compreview-review
lab grade compreview-review
lab finish compreview-review
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Persistência | A especificação exige que a restrição de criação de projetos sobreviva a reinícios do API server | Anote `autoupdate=false` no binding antes de editar os sujeitos (lição 6.5) |
| Nomes dos objetos do template | O enunciado fixa um nome único para todos os recursos criados com o projeto (o `lab grade` confere) | Use exatamente o nome indicado em binding, quota, limit range e policy |
| Label do projeto | O label com o nome do projeto deve estar no objeto `Project` do template, com `${PROJECT_NAME}` | Teste no rascunho rotulando o namespace manualmente, como a solução sugere |
| Valores de CPU e memória | Valores em formatos diferentes (`0.1`, `100m`, `750Mi`, `1Gi`) e "usa" × "pede" | "Usa" = limits; "pede" = requests; mantenha `max ≥ default ≥ defaultRequest` |
| Network policy | Deve aceitar o próprio projeto **ou** o router | Dois itens em `from` (OU); o router é identificado pelo label de ingress do namespace |
| Projeto de teste cedo demais | Criar o projeto antes do rollout do `openshift-apiserver` gera projeto sem os objetos do template | Aguarde o Pod novo do `openshift-apiserver`; se errou, apague o projeto e crie de novo |
| Usuário de cada passo | Grupos, bindings e template são do administrador; o projeto é criado pelo apresentador; o grupo de participantes é criado pelo suporte | Confira `oc whoami` antes de cada etapa |
| Teste da proibição | O participante não deve conseguir criar projetos | `oc new-project` como ele deve falhar com `Forbidden` |

**Verificação conceitual:** só os grupos autorizados criam projetos (inclusive após
reinício do API server); o projeto novo vem com binding, label, quota, limit range e policy
com os nomes e valores pedidos; o grupo de participantes edita o projeto, mas não cria
projetos.

**Diagnóstico se algo falhar:** saída do `lab grade` (traz dicas) → `oc get groups` →
`oc describe clusterrolebinding self-provisioners` → `oc get template -n openshift-config`
→ `oc get projects.config.openshift.io cluster -o yaml` → `oc get
rolebinding,quota,limitrange,networkpolicy -n <projeto>` → `oc get ns <projeto>
--show-labels` → login como cada usuário e `oc new-project` de teste.

---

## 10.3 Lab: Secure Applications

| Campo | Valor |
|-------|-------|
| Seção | 10.3 |
| Tipo | Lab de revisão |
| Identificador `lab` | `compreview-apps` |
| Avaliação | **`lab grade compreview-apps`** |
| Referência | PDF p. 446–458 |

**Competência praticada (resumo autoral):** criar um namespace de suporte com label, quota
e limit range (fora do template de projeto) e dar a um grupo um papel de cluster; preparar
um CronJob de limpeza de projetos com SA própria, ClusterRole sob medida e
ClusterRoleBinding; implantar um banco e uma API com TLS de ponta a ponta (Secret TLS
montado, variáveis, probes HTTPS, Service na 443, Route passthrough com certificado
assinado por CA própria); restringir banco e API com network policies.

**Domine antes:** [4.1](../04-seguranca-de-rede/4.1-tls-no-trafego-externo.md) ·
[4.3](../04-seguranca-de-rede/4.3-network-policies.md) ·
[6.1](../06-autosservico/6.1-quotas-de-projeto-e-cluster.md) ·
[6.3](../06-autosservico/6.3-limit-ranges.md) ·
[8.3](../08-seguranca-aplicacoes/8.3-acesso-de-aplicacoes-a-api.md) ·
[8.5](../08-seguranca-aplicacoes/8.5-manutencao-com-cron-jobs.md).

```bash
lab start compreview-apps
lab grade compreview-apps
lab finish compreview-apps
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| `oc new-project` × `oc create namespace` | O template de projeto do lab anterior está ativo; criar o namespace de suporte como projeto aplica quota, limit range e policies dos workshops | Crie com `oc create namespace`, como a especificação manda |
| Nomes da quota e do limit range | O enunciado fixa o nome para avaliação | Use exatamente o nome indicado |
| Limpeza automática | O CronJob apaga projetos com o label de workshop depois de poucos segundos — inclusive projetos de workshop que você criar para testar | Esperado; não conte com projetos de workshop durante o lab |
| Secret TLS | `oc create secret tls` grava as chaves `tls.crt` e `tls.key`; as variáveis do Deployment apontam para esses nomes no diretório de montagem | Monte o Secret no diretório pedido e confira os caminhos |
| Probes | Com TLS ligado, probes HTTP passam a falhar | Use `scheme: HTTPS` nas três probes |
| Portas | Service na 443 encaminha para a 8080 do Pod | `--port 443 --target-port 8080` |
| Teste com `curl` | Sem a CA própria, a verificação do certificado falha | Use `--cacert` com o arquivo da CA do diretório do lab |
| Network policies por último | Depois de aplicadas, testes de conectividade de Pods sem os labels certos **devem** falhar | Teste a aplicação antes; depois confirme que só o permitido passa |
| Policy da API | Route passthrough entrega o tráfego pelo router | Libere o namespace do router (label de ingress) na porta do Pod |
| Policy do banco | Exige Pod com o label do app **e** namespace com o label do suporte | Mesmo item em `from` (E) |

**Verificação conceitual:** o namespace de suporte tem label, quota e limit range; o
CronJob roda a cada minuto com a SA certa e remove projetos de workshop vencidos; a API
responde por HTTPS com o certificado próprio (verificado pela CA) e grava/consulta o banco;
só a API alcança o banco e só o router alcança a API.

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc get ns <ns> --show-labels` →
`oc describe quota,limitrange -n <ns>` → `oc get cronjob,jobs,pods -n <ns>` e `oc logs` →
`oc auth can-i delete namespaces --as system:serviceaccount:<ns>:<sa>` → `oc describe pod`
(probes) → `oc get route,svc,endpoints -n <ns>` → `oc get networkpolicy -n <ns> -o yaml`.

---

## 10.4 Lab: Deploy Packaged Applications

| Campo | Valor |
|-------|-------|
| Seção | 10.4 |
| Tipo | Lab de revisão |
| Identificador `lab` | `compreview-package` |
| Avaliação | **`lab grade compreview-package`** |
| Referência | PDF p. 459–467 |

**Competência praticada (resumo autoral):** adicionar um repositório Helm e instalar uma
versão fixa de um chart em dois ambientes, com arquivos de valores diferentes; criar um
banco a partir de um template do namespace `openshift`; criar um overlay Kustomize de
produção sobre uma base fornecida, com labels/anotações, réplicas, recursos, um
PodDisruptionBudget e variáveis de ambiente vindas de um Secret.

**Domine antes:** [1.1](../01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) ·
[1.3](../01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) ·
[2.1](../02-aplicacoes-empacotadas/2.1-templates-do-openshift.md) ·
[2.3](../02-aplicacoes-empacotadas/2.3-charts-do-helm.md).

```bash
lab start compreview-package
lab grade compreview-package
lab finish compreview-package
```

**Armadilhas do ambiente:**

| Ponto | Quando vira problema | Como agir |
|-------|----------------------|-----------|
| Versão do chart | Sem `--version`, o Helm instala a mais nova do repositório | Fixe a versão pedida |
| Chaves de valores | Nomes de chaves dependem do chart | Consulte `helm show values <repo>/<chart> --version <v>` antes de escrever os arquivos |
| Template de banco | Fica no namespace `openshift`; parâmetros com nomes próprios | `oc process --parameters -n openshift <template>` para conferir os nomes; senha no enunciado |
| Labels × anotações | A tabela do enunciado lista chaves sem dizer qual é label e qual é anotação | Siga o `kustomization.yaml` fornecido e o texto de verificação do enunciado |
| Base do overlay | O overlay precisa referenciar a base fornecida por caminho relativo | Confira com `oc kustomize <overlay>` antes de aplicar |
| Variáveis do banco | Usuário e senha vêm do Secret criado pelo template, não de valores literais | `valueFrom.secretKeyRef` com as chaves do Secret |
| Helm depois do overlay | Um `helm upgrade` da release de produção depois do overlay reaplica os valores do chart (por exemplo, o número de réplicas) | Não reexecute o Helm em produção depois de aplicar o overlay |
| Contagem de Pods | O banco também roda Pods no projeto de produção | Conte só os Pods da aplicação, como o PDF faz |

**Verificação conceitual:** as duas releases respondem nas URLs pedidas; produção tem o
banco, o PodDisruptionBudget, o número de réplicas e os recursos do overlay, e os Pods têm
as variáveis apontando para o banco.

**Diagnóstico se algo falhar:** saída do `lab grade` → `helm list -A` → `helm get values
<release> -n <ns>` → `oc kustomize <overlay>` → `oc get deploy,pdb,pods -n <ns>` →
`oc set env deployment/<d> --list -n <ns>` → `oc get route -n <ns>`.

---

**Registro:** anote cada lab, com o tempo gasto, no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 10.1 Resumo dos objetivos](10.1-resumo-dos-objetivos-de-aprendizagem.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
