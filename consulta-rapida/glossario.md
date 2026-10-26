# Glossário

> Cobertura: **Capítulos 1–9**. Termos do DO180 no
> [glossário do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/consulta-rapida/glossario.md).

| Termo | Definição |
|-------|-----------|
| **admin / edit / view** | Papéis padrão de projeto: gerenciar tudo (inclusive acessos) / alterar recursos de aplicação / só ler |
| **AdminNetworkPolicy (ANP)** | Política de rede de cluster (administrador), avaliada antes das NetworkPolicies, com ações `Allow`/`Deny`/`Pass` |
| **allowNameChange** | Opção de um patch do Kustomize que permite a um strategic merge patch renomear o recurso |
| **alm-examples** | Anotação do CSV com exemplos de CRs em JSON (os mesmos que a console carrega no editor) |
| **anyuid** | SCC que permite rodar com qualquer UID, inclusive root |
| **APIRemovedInNextReleaseInUse** | Alerta: uma API que a próxima versão do OpenShift remove está em uso (há também a variante `...NextEUSRelease...`) |
| **APIRequestCount** | Recurso do OpenShift que contabiliza requisições por recurso/versão de API e indica quem usa APIs obsoletas |
| **AppliedClusterResourceQuota** | Visão, por namespace, de uma ClusterResourceQuota que o afeta; legível pelo administrador do projeto (não aceita `-A`) |
| **appVersion** | Campo do `Chart.yaml` com a versão da aplicação empacotada (informativo) |
| **Autenticação** | Etapa que identifica quem faz a requisição (token OAuth ou certificado X.509) |
| **Autorização** | Etapa que decide se o sujeito pode executar a ação (RBAC) |
| **autoupdate (anotação)** | `rbac.authorization.kubernetes.io/autoupdate`: com `true`, o API server restaura papéis e bindings protegidos ao iniciar |
| **Base (Kustomize)** | Diretório com os recursos comuns a todos os ambientes e seu `kustomization.yaml` |
| **BaselineAdminNetworkPolicy (BANP)** | Política de rede de cluster de menor precedência, que as NetworkPolicies podem sobrepor |
| **basic-user** | Papel padrão que permite obter informações básicas sobre projetos e usuários |
| **behavior (generator)** | `create`, `merge` ou `replace`: como um generator do overlay trata um objeto gerado pela base |
| **Boot image skew** | Defasagem da imagem de boot dos nós; abaixo do mínimo, o MCO bloqueia atualizações (4.22) |
| **Canal de atualização** | `candidate`, `fast`, `stable` ou `eus`-4.x: define as versões que o cluster pode receber |
| **CatalogSource** | Recurso do OLM (em `openshift-marketplace`) que aponta uma imagem de catálogo de operators |
| **Chart (Helm)** | Pacote com `Chart.yaml`, `values.yaml` e `templates/` que descreve uma aplicação |
| **Chart.yaml** | Metadados de um chart: nome, `version`, `appVersion`, descrição |
| **Chave de mesclagem** | Campo que identifica itens de uma lista no strategic merge patch (ex.: `name` em containers, `mountPath` em volumeMounts) |
| **Cluster Samples Operator** | Operator que popula templates e image streams de exemplo no namespace `openshift` |
| **Cluster Version Operator (CVO)** | Operator que instala e atualiza os cluster operators junto com a versão do OpenShift |
| **cluster-admin** | Papel com acesso irrestrito a todo o cluster |
| **cluster-reader** | Papel de leitura da maioria dos objetos do cluster |
| **ClusterExtension** | Recurso do OLM v1 que instala e atualiza um operator de forma declarativa (versão, faixa ou canal) |
| **ClusterResourceQuota** | Quota do OpenShift (`quota.openshift.io/v1`) que limita a soma de vários namespaces escolhidos por selector |
| **ClusterRole** | Papel definido no escopo do cluster; reutilizável em projetos por RoleBindings |
| **ClusterRoleBinding** | Vínculo de um papel a sujeitos em todo o cluster |
| **ClusterServiceVersion (CSV)** | Recurso de namespace que descreve uma versão de um operator (Deployments, permissões, CRDs) e o estado da instalação |
| **ClusterUserDefinedNetwork (CUDN)** | Rede definida pelo administrador do cluster que une vários namespaces selecionados por labels |
| **ClusterVersion** | Recurso de cluster (`version`) com versão, canal, condições e histórico de atualizações |
| **commonLabels** | Campo obsoleto do Kustomize; adiciona labels também a selectors e templates |
| **configMapGenerator** | Gera ConfigMaps a partir de `literals`, `envs` ou `files`, com hash no nome |
| **`count/<recurso>.<grupo>`** | Sintaxe de quota de contagem de objetos; recursos do grupo core dispensam o `.<grupo>` |
| **CronJob** | Recurso `batch/v1` que cria Jobs conforme um agendamento no formato cron |
| **Custom resource (CR)** | Instância de um tipo definido por uma CRD; é como se pede algo a um operator |
| **CustomResourceDefinition (CRD)** | Extensão da API que cria um tipo novo de objeto |
| **Declarativo (fluxo)** | Manter o estado desejado em arquivos versionados e aplicá-lo com `oc apply` |
| **default / defaultRequest** | Chaves do LimitRange com o limit e o request injetados em containers que não os declaram |
| **default-deny** | NetworkPolicy com `podSelector: {}` e sem regras, que isola todos os Pods do namespace |
| **Drift** | Diferença entre o que está no Git e o que está no cluster, causada por mudança fora do fluxo |
| **Dry run** | Execução que não persiste nada; `client` (local) ou `server` (validado pelo API server) |
| **Edge (terminação)** | TLS termina no router; o trecho router → Pod segue em texto claro |
| **EUS (Extended Update Support)** | Versões menores pares com ciclo de suporte estendido e canal `eus-4.x` |
| **generatorOptions** | Opções de todos os generators de uma kustomization (`disableNameSuffixHash`, `labels`, `annotations`) |
| **GitOps** | Prática em que o Git é a fonte da verdade do estado do cluster e ferramentas reconciliam as diferenças |
| **Group** | Conjunto de usuários que recebe permissões de uma vez |
| **Helm** | Ferramenta que instala, atualiza e reverte aplicações empacotadas como charts |
| **HelmChartRepository / ProjectHelmChartRepository** | Recursos que publicam um repositório de charts na console (cluster inteiro / um projeto) |
| **Hook (Helm)** | Tarefa que o chart executa em momentos do ciclo (antes/depois de instalar ou atualizar) |
| **host-device** | Plug-in CNI que move uma interface do nó para um único Pod |
| **HTPasswd (IdP)** | Provedor de identidade que valida usuários contra um arquivo htpasswd guardado em Secret |
| **Identity provider (IdP)** | Sistema que valida credenciais para o servidor OAuth (HTPasswd, LDAP, OIDC...) |
| **Identity** | Registro do vínculo de um User com um provedor de identidade (`idp:usuario`) |
| **Imperativo (fluxo)** | Alterar o cluster com uma sequência de comandos de ação |
| **Ingress controller** | Componente (router) que recebe o tráfego externo e o encaminha aos Pods conforme Routes/Ingress |
| **inject-cabundle** | Anotação `service.beta.openshift.io/inject-cabundle=true` que faz o service-ca injetar sua CA em um objeto |
| **InstallPlan** | Plano do OLM para instalar ou atualizar um operator; com aprovação manual exige `spec.approved: true` |
| **installPlanApproval** | Campo da Subscription: `Automatic` ou `Manual` |
| **IPAddressPool** | Recurso do MetalLB com a faixa de IPs que pode ser atribuída a Services LoadBalancer |
| **Job** | Recurso `batch/v1` que executa uma tarefa até concluir |
| **JSON Patch** | Formato (RFC 6902) de operações `op`/`path`/`value` em caminhos exatos |
| **kubeadmin** | Usuário virtual de administrador criado na instalação; senha em hash no Secret `kubeadmin` (`kube-system`) |
| **kustomization.yaml** | Arquivo que define recursos, transformações, patches e generators de um diretório Kustomize |
| **Kustomize** | Ferramenta que produz variações de manifestos YAML sem templates; integrada ao `oc` (`-k`, `oc kustomize`) |
| **L2Advertisement** | Recurso do MetalLB que anuncia IPs dos pools por ARP (camada 2) |
| **last-applied-configuration** | Anotação `kubectl.kubernetes.io/last-applied-configuration` com o último manifesto aplicado |
| **LimitRange** | Objeto de namespace com padrões, mínimos e máximos de recursos por container, Pod ou PVC; aplicado na criação do Pod |
| **LoadBalancer (Service)** | Tipo de Service que recebe um IP externo dedicado, fornecido pela nuvem ou pelo MetalLB |
| **Machine Config Operator (MCO)** | Operator que aplica configuração e SO nos nós e faz o rolling reboot nas atualizações |
| **MachineHealthCheck (MHC)** | Recurso que reinicia máquinas não saudáveis; deve ser pausado durante atualizações |
| **MACVLAN / IPVLAN** | Plug-ins CNI que criam interfaces virtuais sobre uma interface do nó (com MAC próprio / MAC compartilhado) |
| **Manifesto de recurso** | Arquivo YAML ou JSON que descreve o estado desejado de um ou mais objetos |
| **mappingMethod** | Campo do IdP que define como identidades são ligadas a Users (padrão `claim`) |
| **MetalLB** | Operador que implementa Services LoadBalancer fora da nuvem, por ARP (L2) ou BGP (L3) |
| **ML-KEM** | Mecanismo pós-quântico de troca de chaves, usado de forma híbrida em TLS 1.3 no 4.22 |
| **Multus** | Plug-in CNI que permite várias interfaces de rede por Pod |
| **namePrefix / nameSuffix** | Prefixo/sufixo aplicado pelo Kustomize ao nome de todos os recursos |
| **NetworkAttachmentDefinition (NAD)** | Recurso de namespace com a configuração CNI de uma rede adicional |
| **NetworkPolicy** | Recurso de namespace que define, por labels, de onde os Pods podem receber (e para onde enviar) tráfego |
| **NodePort** | Tipo de Service exposto em uma porta alta em todos os nós |
| **OAuth (recurso)** | Objeto `oauth/cluster` (`config.openshift.io/v1`) que lista os provedores de identidade |
| **OAuth server** | Servidor de autenticação do OpenShift (Pods `oauth-openshift`) que emite tokens de acesso |
| **openshift.io/required-scc** | Anotação do template de Pod que fixa a SCC exigida |
| **openshift.io/scc** | Anotação do Pod com a SCC aplicada na admissão |
| **Operator** | Controller específico de uma aplicação que observa CRs e mantém os objetos correspondentes |
| **Operator Lifecycle Manager (OLM)** | Componente que instala, atualiza e remove add-on operators a partir de catálogos |
| **OperatorGroup** | Recurso que define os namespaces observados pelos operators do seu namespace; um por namespace |
| **OTA (over-the-air)** | Atualização do cluster pela rede a partir de imagens de release e de um grafo de caminhos |
| **Overlay** | Diretório Kustomize que referencia uma base e declara as diferenças de um ambiente |
| **PackageManifest** | Recurso criado pelo OLM para cada operator disponível, com canais, versões e modos de instalação |
| **Parâmetro (template)** | Lacuna de um template, com valor padrão, obrigatoriedade ou gerador |
| **Passthrough (terminação)** | O router não decifra; o TLS vai até o Pod, que apresenta o próprio certificado |
| **privileged (SCC)** | SCC que remove quase todas as restrições; acesso total ao nó |
| **Processar (template)** | Substituir os parâmetros e obter a lista de manifestos (`oc process`) |
| **Project (config.openshift.io)** | Configuração global de projetos (`cluster`): `projectRequestTemplate` e `projectRequestMessage` |
| **ProjectRequest** | Pedido de criação de projeto (`oc new-project`); o API server do OpenShift cria o namespace a partir do template de projeto |
| **RBAC** | Role-based access control: permissões agrupadas em papéis e ligadas a sujeitos por vínculos |
| **Re-encrypt (terminação)** | TLS termina no router, que abre nova conexão TLS até o Pod |
| **Release (Helm)** | Instalação nomeada de um chart em um namespace |
| **ResourceQuota** | Objeto de namespace que limita a soma de requests/limits e a contagem de objetos |
| **restricted-v2** | SCC padrão: UID aleatório da faixa do projeto, sem root e sem acesso ao host |
| **Revisão (Helm)** | Cada estado de uma release (install, upgrade, rollback), listado em `helm history` |
| **Role** | Papel definido em um projeto |
| **RoleBinding** | Vínculo de um papel (Role ou ClusterRole) a sujeitos em um projeto |
| **router-ca** | Secret em `openshift-ingress-operator` com a CA interna que assina os certificados padrão das Routes |
| **SAN (Subject Alternative Name)** | Extensão do certificado com os nomes válidos; é o que os clientes verificam |
| **scc-subject-review** | Subcomando de `oc adm policy` que indica quais SCCs permitiriam um workload |
| **secretGenerator** | Como o `configMapGenerator`, mas gera Secrets e faz a codificação base64 |
| **Security context constraints (SCC)** | Mecanismo do OpenShift que limita os privilégios dos Pods no host; concedido a service accounts |
| **self-provisioner** | ClusterRole que permite criar projetos; concedida por padrão, pelo binding `self-provisioners`, ao grupo `system:authenticated:oauth` |
| **service-ca** | Controller do OpenShift que emite certificados para Services anotados e distribui sua CA |
| **ServiceAccount** | Identidade de aplicações (`system:serviceaccount:<ns>:<nome>`) |
| **serving-cert-secret-name** | Anotação de Service que pede ao service-ca um Secret TLS com certificado assinado |
| **SNI** | Extensão do TLS que informa o nome do host no handshake; permite rotear TLS por nome |
| **Strategic merge patch** | Patch com a estrutura do recurso; mescla listas por chave de mesclagem; padrão do `oc patch` |
| **Subscription** | Pedido de instalação de um operator: pacote, catálogo, canal e política de aprovação |
| **system:anonymous** | Usuário virtual atribuído a requisições sem credencial |
| **Template (OpenShift)** | Recurso `template.openshift.io/v1` com objetos parametrizados |
| **Template de projeto** | Template (`project-request`, em `openshift-config`) com os objetos que todo projeto novo recebe |
| **Three-way merge** | Comparação feita pelo `oc apply` entre manifesto, objeto vivo e `last-applied-configuration` |
| **TokenRequest** | API que emite tokens de service account com validade limitada (usada pelo kubelet e por `oc create token`) |
| **Update channel** | Sequência de versões de um operator que a Subscription acompanha (ex.: `stable`) |
| **User** | Ator que interage com a API; criado no primeiro login |
| **UserDefinedNetwork (UDN)** | Rede de um namespace gerenciada pelo OVN-Kubernetes, primária ou secundária |
| **values.yaml** | Valores padrão de um chart; sobrescritos por `-f` e `--set` |
| **Zero trust** | Modelo em que nenhuma comunicação é confiável por padrão: tudo é autorizado, cifrado e verificado |

---
[Início](../README.md)
