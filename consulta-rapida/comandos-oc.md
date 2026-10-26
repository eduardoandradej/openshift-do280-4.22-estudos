# Comandos `oc` por tarefa

> Cobertura: **Capítulos 1–3**. Os comandos do DO180 estão na
> [consulta rápida do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/consulta-rapida/comandos-oc.md).
> Coluna **Tipo:** 🔍 somente leitura · ✏️ altera o cluster · 🖥️ só local.

## Manifestos (Cap. 1)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc create <tipo> <nome> ... --dry-run=client -o yaml > arq.yaml` | Gerar esqueleto de manifesto | 🖥️ | [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) |
| `... --save-config` | Incluir a anotação `last-applied-configuration` | 🖥️/✏️ | 1.1 |
| `oc explain <tipo>.<campo> [--recursive]` | Documentação de campos | 🔍 | 1.1 |
| `oc create -f <arq\|dir\|url\|->` | Criar (erro se existir) | ✏️ | 1.1 |
| `oc create -R -f <dir>` | Criar percorrendo subdiretórios | ✏️ | 1.1 |
| `oc apply -f <dir> --dry-run=server --validate=true` | Validar no servidor sem persistir | 🔍 | 1.1 |
| `oc diff -f <dir>` | Diferença entre vivo e resultado do apply | 🔍 | 1.1 |
| `oc apply -f <dir>` | Criar ou atualizar | ✏️ | 1.1 |
| `oc apply view-last-applied <tipo>/<nome>` | Ver o último manifesto aplicado (📘) | 🔍 | 1.1 |
| `oc rollout restart deployment/<d>` | Recriar Pods após mudar ConfigMap/Secret | ✏️ | 1.1 |
| `oc patch <tipo> <nome> -p '<json>'` | Strategic merge patch inline | ✏️ | 1.1 |
| `oc patch <tipo> <nome> --patch-file <arq>` | Patch a partir de arquivo | ✏️ | 1.1 |
| `oc patch <tipo> <nome> --type json -p '[...]'` | JSON Patch (📘 também `--type merge`) | ✏️ | 1.1 |
| `oc delete -f <dir>` | Remover o que os manifestos descrevem | ✏️ | 1.1 |

## Kustomize (Cap. 1)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc kustomize <dir>` | Renderizar base/overlay | 🖥️ | [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) |
| `oc apply -k <dir> --dry-run=server` | Validar o resultado no cluster | 🔍 | 1.3 |
| `oc diff -k <dir>` | Comparar o resultado com o vivo (📘) | 🔍 | 1.3 |
| `oc apply -k <dir>` | Renderizar e aplicar | ✏️ | 1.3 |
| `oc delete -k <dir>` | Remover o que o diretório gera | ✏️ | 1.3 |
| `oc extract secret/<nome-hash> --to -` | Ler um Secret gerado | 🔍 | 1.3 |
| `oc get deployment <d> -o jsonpath='{.spec.template.spec.containers[0].envFrom}'` | Conferir qual ConfigMap/Secret o Deployment usa | 🔍 | 1.3 |

## Templates do OpenShift (Cap. 2)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc get templates -n openshift` | Catálogo global (Samples Operator) | 🔍 | [2.1](../capitulos/02-aplicacoes-empacotadas/2.1-templates-do-openshift.md) |
| `oc describe template <t> -n <ns>` | Objetos, parâmetros e labels | 🔍 | 2.1 |
| `oc process --parameters <t> -n <ns>` · `-f arq.yaml` | Parâmetros, padrões e geradores | 🔍 | 2.1 |
| `oc get template <t> -n <ns> -o yaml > arq.yaml` | Copiar um template | 🔍 | 2.1 |
| `oc create -f arq.yaml [-n <ns>]` | Publicar o template no projeto | ✏️ | 2.1 |
| `oc new-app --template <t> -p K=V` | Instanciar em um passo | ✏️ | 2.1 |
| `oc process <t> -p K=V -o yaml > m.yaml` | Gerar manifestos (padrão: JSON) | 🔍 | 2.1 |
| `oc process <t> --param-file p.txt \| oc diff -f -` | Prever atualização | 🔍 | 2.1 |
| `oc process <t> --param-file p.txt \| oc apply -f -` | Criar ou atualizar | ✏️ | 2.1 |

## Helm (Cap. 2)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `helm repo add <nome> <url>` · `list` · `update` · `remove` | Repositórios (config. local) | 🖥️ | [2.3](../capitulos/02-aplicacoes-empacotadas/2.3-charts-do-helm.md) |
| `helm search repo [<termo>] --versions` | Charts e versões | 🖥️ | 2.3 |
| `helm show chart\|values\|readme <chart> [--version X]` | Inspecionar chart | 🖥️ | 2.3 |
| `helm template <r> <chart> -f v.yaml` | Renderizar localmente (📘) | 🖥️ | 2.3 |
| `helm install <r> <chart> -f v.yaml --version X --dry-run` | Prévia da instalação | 🔍 | 2.3 |
| `helm install <r> <chart> -f v.yaml --version X [-n ns]` | Instalar (cria release) | ✏️ | 2.3 |
| `helm list [-n ns\|--all-namespaces]` | Releases | 🔍 | 2.3 |
| `helm get values <r> [--all]` · `helm get manifest <r>` | Valores e manifestos da release (📘) | 🔍 | 2.3 |
| `helm upgrade <r> <chart> -f v.yaml --version Y` | Atualizar (nova revisão) | ✏️ | 2.3 |
| `helm history <r>` | Revisões | 🔍 | 2.3 |
| `helm rollback <r> <rev>` | Voltar a uma revisão | ✏️ | 2.3 |
| `helm uninstall <r>` | Remover release e objetos | ✏️ | 2.3 |

## Autenticação (Cap. 3)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `export KUBECONFIG=<arq>` · `oc --kubeconfig <arq> ...` | Usar o kubeconfig de administrador | 🔍 | [3.1](../capitulos/03-autenticacao-autorizacao/3.1-provedor-de-identidade-htpasswd.md) |
| `htpasswd -c -B -b <arq> <u> '<senha>'` | Criar o arquivo (apaga se existir) | 🖥️ | 3.1 |
| `htpasswd -B -b <arq> <u> '<senha>'` · `htpasswd -D <arq> <u>` | Incluir/alterar · remover usuário | 🖥️ | 3.1 |
| `oc create secret generic <s> --from-file htpasswd=<arq> -n openshift-config` | Criar o Secret do IdP | ✏️ | 3.1 |
| `oc extract secret/<s> -n openshift-config --to <dir> --confirm` | Trazer o arquivo atual | 🔍 | 3.1 |
| `oc set data secret/<s> -n openshift-config --from-file htpasswd=<arq>` | Devolver o arquivo atualizado | ✏️ | 3.1 |
| `oc get oauth cluster -o yaml > o.yaml` · `oc replace -f o.yaml` | Editar a lista de IdPs | ✏️ | 3.1 |
| `oc patch oauth cluster --type json -p '[{"op":"add","path":"/spec/identityProviders/-","value":{...}}]'` | Acrescentar IdP sem apagar os outros (📘) | ✏️ | 3.1 |
| `watch oc get pods -n openshift-authentication` · `oc get co authentication` | Acompanhar o OAuth | 🔍 | 3.1 |
| `oc get users` · `oc get identities` | Usuários e identidades | 🔍 | 3.1 |
| `oc delete user <u>` · `oc delete identity <idp>:<u>` | Remover usuário do cluster | ✏️ | 3.1 |
| `oc delete secret kubeadmin -n kube-system` | Remover o kubeadmin (nunca no curso) | ✏️ | 3.1 |

## Autorização / RBAC (Cap. 3)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc adm policy add-cluster-role-to-user <papel> <u>` | ClusterRoleBinding para usuário | ✏️ | [3.3](../capitulos/03-autenticacao-autorizacao/3.3-permissoes-com-rbac.md) |
| `oc adm policy remove-cluster-role-from-user <papel> <u>` | Remover papel de cluster | ✏️ | 3.3 |
| `oc adm policy add-cluster-role-to-group <papel> <g>` | ClusterRoleBinding para grupo | ✏️ | 3.3 |
| `oc policy add-role-to-user <papel> <u> -n <p>` | RoleBinding no projeto | ✏️ | 3.3 |
| `oc policy add-role-to-group <papel> <g> -n <p>` | RoleBinding para grupo | ✏️ | 3.3 |
| `oc policy remove-role-from-user\|group <papel> <s> -n <p>` | Remover papel no projeto | ✏️ | 3.3 |
| `oc adm groups new <g> [usuarios]` · `add-users` · `remove-users` | Grupos locais | ✏️ | 3.3 |
| `oc get groups` | Grupos e membros | 🔍 | 3.3 |
| `oc get rolebindings -o wide -n <p>` · `oc get clusterrolebindings -o wide` | Quem tem qual papel | 🔍 | 3.3 |
| `oc describe clusterrole <papel>` | Regras de um papel | 🔍 | 3.3 |
| `oc adm policy who-can <verbo> <recurso> [-n <p>]` | Quem pode | 🔍 | 3.3 |
| `oc auth can-i <verbo> <recurso> [-n <p>] [--as <u>] [--list]` | Testar permissão (📘) | 🔍 | 3.3 |
| `oc create role\|rolebinding ...` | RBAC imperativo (📘) | ✏️ | 3.3 |

## Git nas atividades (apoio)

| Comando | Para quê |
|---------|----------|
| `git clone <url> --branch <b>` | Clonar já na versão desejada |
| `git branch -a` · `git log --oneline` | Ver branches, tags e versões |
| `git checkout <b>` | Trocar de versão |
| `git show` | Ver o que mudou no último commit |

---
[Início](../README.md)
