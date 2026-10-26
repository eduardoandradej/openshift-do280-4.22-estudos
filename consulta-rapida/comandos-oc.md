# Comandos `oc` por tarefa

> Cobertura: **Capítulos 1–8**. Os comandos do DO180 estão na
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

## TLS em Routes (Cap. 4)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc expose service <s> --hostname <h>` | Route HTTP (sem TLS) | ✏️ | [4.1](../capitulos/04-seguranca-de-rede/4.1-tls-no-trafego-externo.md) |
| `oc create route edge <r> --service <s> --hostname <h> [--key k --cert c]` | TLS termina no router | ✏️ | 4.1 |
| `oc create route passthrough <r> --service <s> --port <p> --hostname <h>` | TLS termina no Pod | ✏️ | 4.1 |
| `oc create route reencrypt <r> --service <s> --hostname <h> [--key --cert]` | TLS no router + novo TLS até o Pod | ✏️ | 4.1 |
| `oc create route edge <r> --service <s> --dry-run=client -o yaml` | Gerar manifesto (o `--dry-run=server` cria a Route!) | 🔍 | 4.1 |
| `oc get secret router-ca -n openshift-ingress-operator -o yaml` | CA interna do ingress | 🔍 | 4.1 |
| `oc create secret tls <s> --cert <crt> --key <key>` | Secret TLS para a aplicação | ✏️ | 4.1 |
| `oc set volumes deployment/<d>` | Ver volumes montados | 🔍 | 4.1 |
| `oc debug -t deployment/<d> --image <img>` | Pod temporário de diagnóstico | ✏️ | 4.1 |
| `curl -v --cacert <ca> https://<h>` | Ver e validar o certificado | 🔍 | 4.1 |

## NetworkPolicies (Cap. 4)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc get networkpolicy [-n <ns>]` | Listar políticas | 🔍 | [4.3](../capitulos/04-seguranca-de-rede/4.3-network-policies.md) |
| `oc describe networkpolicy <np>` | Regra em linguagem legível (E × OU) | 🔍 | 4.3 |
| `oc apply -f <np>.yaml -n <ns-destino>` | Aplicar política no namespace do destino | ✏️ | 4.3 |
| `oc label namespace <ns> chave=valor` | Rotular namespace de origem | ✏️ | 4.3 |
| `oc get ns --show-labels` · `oc get pods --show-labels` | Conferir labels usados nos selectors | 🔍 | 4.3 |
| `oc exec <pod> -- curl -s -m 5 <destino>` | Testar conectividade com tempo limite | 🔍 | 4.3 |
| `oc get adminnetworkpolicies` | Políticas de cluster (ANP) | 🔍 | 4.3 |

## TLS interno / service-ca (Cap. 4)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc annotate service <s> service.beta.openshift.io/serving-cert-secret-name=<secret>` | Gerar certificado de serviço | ✏️ | [4.5](../capitulos/04-seguranca-de-rede/4.5-tls-no-trafego-interno.md) |
| `oc get secret <secret> -o jsonpath='{.data.tls\.crt}' \| base64 -d \| openssl x509 -noout -text` | Ler o certificado (SAN, emissor) | 🔍 | 4.5 |
| `oc create configmap <cm>` + `oc annotate configmap <cm> service.beta.openshift.io/inject-cabundle=true` | Injetar a CA (ConfigMap dedicado) | ✏️ | 4.5 |
| `oc exec deploy/<d> -- openssl s_client -connect <svc>.<ns>.svc:443` | Verificar a cadeia de dentro do cluster | 🔍 | 4.5 |
| `oc delete secret <secret>` | Rotacionar o certificado de um Service | ✏️ | 4.5 |

## Services LoadBalancer (Cap. 5)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc expose deployment/<d> --type LoadBalancer [--name <s>] [--port <p>] [--target-port <tp>]` | Expor protocolo não HTTP com IP próprio | ✏️ | [5.1](../capitulos/05-redes-nao-http/5.1-servicos-load-balancer.md) |
| `oc get svc <s> -o jsonpath='{.status.loadBalancer.ingress[0].ip}'` | IP externo atribuído | 🔍 | 5.1 |
| `oc get svc -A \| grep LoadBalancer` | Quem está ocupando o pool | 🔍 | 5.1 |
| `oc get metallb,ipaddresspool,l2advertisement -n metallb-system` | Configuração do MetalLB (administrador) | 🔍 | 5.1 |
| `nc -vz <ip> <porta>` | Testar se a porta aceita conexão | 🔍 | 5.1 |

## Redes adicionais (Cap. 5)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc create -f <namespace-com-label>.yaml` | Namespace para UDN primária (label só na criação) | ✏️ | [5.3](../capitulos/05-redes-nao-http/5.3-redes-adicionais-de-pods.md) |
| `oc apply -f udn.yaml` · `oc get userdefinednetwork -n <ns> -o yaml` | Criar UDN e ver condições | ✏️ / 🔍 | 5.3 |
| `oc describe clusteruserdefinednetwork <c>` | Estado de uma CUDN | 🔍 | 5.3 |
| `oc get net-attach-def [-A] [-l k8s.ovn.org/user-defined-network]` | NADs (manuais ou geradas por UDN/CUDN) | 🔍 | 5.3 |
| `oc get pod <p> -o jsonpath='{.metadata.annotations.k8s\.v1\.cni\.cncf\.io/network-status}'` | Redes e interfaces do Pod | 🔍 | 5.3 |
| `oc debug node/<nó> -- chroot /host ip addr show <if>` | Interface existe no nó? | 🔍 | 5.3 |

## Quotas e limit ranges (Cap. 6)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc create quota <q> [-n <ns>] --hard requests.cpu=4,limits.memory=8Gi,count/deployments.apps=10` | Criar quota de computação/objetos | ✏️ | [6.1](../capitulos/06-autosservico/6.1-quotas-de-projeto-e-cluster.md) |
| `oc get quota` · `oc describe quota <q>` · `oc get quota <q> -o yaml` | Uso × teto (`status.used`) | 🔍 | 6.1 |
| `oc create clusterresourcequota <c> --project-label-selector k=v --hard ...` | Quota para a soma de vários projetos | ✏️ | 6.1 |
| `oc describe AppliedClusterResourceQuota -n <ns>` | Cluster quota vista pelo projeto (sem `-A`) | 🔍 | 6.1 |
| `oc api-resources --api-group="" --namespaced=true` | Nomes para `count/<recurso>[.<grupo>]` | 🔍 | 6.1 |
| `oc apply -f limitrange.yaml` · `oc describe limitrange` | Criar e consultar limit range | ✏️ / 🔍 | [6.3](../capitulos/06-autosservico/6.3-limit-ranges.md) |
| `oc set resources deployment <d> --requests cpu=250m --limits memory=512Mi` | Declarar recursos de um workload | ✏️ | 6.3 |
| `oc events` | `exceeded quota`, `must specify ...`, `minimum/maximum ... per Container` | 🔍 | 6.1 · 6.3 |

## Template de projeto e self-provisioner (Cap. 6)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc adm create-bootstrap-project-template -o yaml > t.yaml` | Template inicial (comportamento padrão) | 🖥️ | [6.5](../capitulos/06-autosservico/6.5-template-de-projeto-e-self-provisioner.md) |
| `oc get limitrange,resourcequota -n <ns> -o yaml` | Itens para copiar para `objects:` | 🔍 | 6.5 |
| `oc create -f t.yaml -n openshift-config` | Publicar o template | ✏️ | 6.5 |
| `oc edit projects.config.openshift.io cluster` | `spec.projectRequestTemplate.name` / `projectRequestMessage` (⛔ cluster inteiro) | ✏️ | 6.5 |
| `oc get pod -n openshift-apiserver` | Aguardar o rollout após a mudança | 🔍 | 6.5 |
| `oc describe clusterrolebinding.rbac self-provisioners` | Quem pode criar projetos | 🔍 | 6.5 |
| `oc annotate clusterrolebinding/self-provisioners --overwrite rbac.authorization.kubernetes.io/autoupdate=false` | Tornar alterações permanentes | ✏️ | 6.5 |
| `oc patch clusterrolebinding.rbac self-provisioners -p '{"subjects": null}'` | Desligar o autosserviço (⛔ cluster inteiro) | ✏️ | 6.5 |

## Operators (Cap. 7)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc get clusteroperator` · `oc describe co <co>` | Estado dos cluster operators (CVO) | 🔍 | [7.1](../capitulos/07-operadores-kubernetes/7.1-operators-e-olm.md) |
| `oc get catalogsource -n openshift-marketplace` | Catálogos de operators | 🔍 | 7.1 |
| `oc get packagemanifests` · `oc describe packagemanifest <p>` | Operators disponíveis; catálogo, canais, modos, namespace sugerido | 🔍 | [7.5](../capitulos/07-operadores-kubernetes/7.5-instalar-operators-pela-cli.md) |
| `oc create -f operator-group.yaml` · `oc create -f subscription.yaml` | Instalar pela CLI | ✏️ | 7.5 |
| `oc describe operator <pacote>` | Install plan referenciado e condições (`InstallPlanPending`, `InstallSucceeded`) | 🔍 | 7.5 |
| `oc get installplan -n <ns>` | Planos e `APPROVED` | 🔍 | 7.5 |
| `oc patch installplan <ip> -n <ns> --type merge -p '{"spec":{"approved":true}}'` | Aprovar instalação/atualização manual | ✏️ | 7.5 |
| `oc patch subscription <s> -n <ns> --type merge -p '{"spec":{"channel":"<c>"}}'` | Trocar o canal | ✏️ | 7.5 |
| `oc get csv [-A] [-n <ns>]` | Versões instaladas e fase (`Succeeded`) | 🔍 | [7.3](../capitulos/07-operadores-kubernetes/7.3-instalar-operators-pela-console.md) · 7.5 |
| `oc get csv <csv> -o jsonpath='{.spec.customresourcedefinitions.owned[*].name}'` | CRDs do operator | 🔍 | 7.5 |
| `oc get csv <csv> -o jsonpath='{.metadata.annotations.alm-examples}' \| jq` | Exemplos de CR | 🔍 | 7.5 |
| `oc delete subscription <s> -n <ns>` · `oc delete csv <csv> -n <ns>` | Remover pela CLI (📘) | ✏️ | 7.5 |

## Segurança de aplicações (Cap. 8)

| Comando | Para quê | Tipo | Lição |
|---------|----------|------|-------|
| `oc get scc` · `oc describe scc <scc>` | SCCs do cluster | 🔍 | [8.1](../capitulos/08-seguranca-aplicacoes/8.1-security-context-constraints.md) |
| `oc describe pod <p> \| grep scc` | SCC aplicada (`openshift.io/scc`) | 🔍 | 8.1 |
| `oc get deploy <d> -o yaml \| oc adm policy scc-subject-review -f -` | SCC que permitiria o workload | 🔍 | 8.1 |
| `oc create sa <sa>` · `oc set serviceaccount deployment/<d> <sa>` | Criar SA e associá-la (recria os Pods) | ✏️ | 8.1 |
| `oc adm policy add-scc-to-user <scc> -z <sa>` | Conceder SCC (ClusterRoleBinding; administrador) | ✏️ | 8.1 |
| `oc create role <r> --verb use --resource scc --resource-name <scc>` + `oc create rolebinding <rb> --role <r> --serviceaccount <ns>:<sa>` | Conceder SCC só no projeto | ✏️ | 8.1 |
| `oc adm policy add-role-to-user <papel> -z <sa>` | Papel no projeto atual | ✏️ | [8.3](../capitulos/08-seguranca-aplicacoes/8.3-acesso-de-aplicacoes-a-api.md) |
| `oc adm policy add-role-to-user <papel> system:serviceaccount:<proj>:<sa> --rolebinding-name <n> -n <outro>` | Papel em outro projeto | ✏️ | 8.3 |
| `oc adm policy add-cluster-role-to-user <papel> -z <sa>` | Papel em todo o cluster | ✏️ | 8.3 |
| `oc auth can-i <verbo> <recurso> -n <ns> --as system:serviceaccount:<proj>:<sa>` | Testar permissões da SA | 🔍 | 8.3 |
| `oc create token <sa> [--duration 30m]` | Token para uso fora do cluster | ✏️ | 8.3 |
| `oc create cronjob <cj> --image <i> --schedule '<cron>' --dry-run=client -o yaml -- <cmd>` | Esqueleto de CronJob | 🔍 | [8.5](../capitulos/08-seguranca-aplicacoes/8.5-manutencao-com-cron-jobs.md) |
| `oc create job <j> --from cronjob/<cj>` | Executar o CronJob agora | ✏️ | 8.5 |
| `oc get cronjobs,jobs,pods` · `oc logs job/<j>` | Acompanhar execuções | 🔍 | 8.5 |
| `oc debug node/<nó> -- chroot /host crictl images` | Imagens no nó | 🔍 | 8.5 |

## Git nas atividades (apoio)

| Comando | Para quê |
|---------|----------|
| `git clone <url> --branch <b>` | Clonar já na versão desejada |
| `git branch -a` · `git log --oneline` | Ver branches, tags e versões |
| `git checkout <b>` | Trocar de versão |
| `git show` | Ver o que mudou no último commit |

---
[Início](../README.md)
