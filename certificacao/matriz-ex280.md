# Matriz de competências EX280

## Como ler

| Marca | Significado |
|-------|-------------|
| ✅ | Lição do DO280 escrita e validada |
| ⏳ | Lição do DO280 ainda não escrita (aguardando PDF) |
| ➕ | Complementar: ajuda na prática, mas não é objetivo declarado |

## Áreas do exame × material de estudo

| Área do EX280 | DO180 | DO280 |
|---------------|-------|-------|
| Gerenciar o OpenShift: console, CLI, consultar/formatar/filtrar atributos, logs, eventos, diagnóstico de containers, Pods e cluster | [2.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.1-interfaces-de-linha-de-comando.md) · [2.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.3-inspecionar-recursos-kubernetes.md) · [2.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.5-avaliar-a-saude-do-cluster.md) · [3.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/03-containers-e-pods/3.5-diagnosticar-containers-e-pods.md) | Reforço em todos os capítulos (diagnóstico) |
| Implantar aplicações a partir de manifestos e gerenciá-las de forma declarativa (incluindo Kustomize) | [4.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.1-implantar-a-partir-de-imagem-e-template.md) | ✅ [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) · ✅ [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) |
| Implantar aplicações empacotadas (templates, Helm) | [4.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.1-implantar-a-partir-de-imagem-e-template.md) | ✅ [2.1](../capitulos/02-aplicacoes-empacotadas/2.1-templates-do-openshift.md) · ✅ [2.3](../capitulos/02-aplicacoes-empacotadas/2.3-charts-do-helm.md) |
| Gerenciar autenticação (provedor HTPasswd, usuários, grupos) e autorização (RBAC) | — | ✅ [3.1](../capitulos/03-autenticacao-autorizacao/3.1-provedor-de-identidade-htpasswd.md) · ✅ [3.3](../capitulos/03-autenticacao-autorizacao/3.3-permissoes-com-rbac.md) |
| Proteger o tráfego de rede (TLS em Routes, NetworkPolicies, TLS interno) | [4.7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.7-escalar-e-expor-aplicacoes.md) | ✅ [4.1](../capitulos/04-seguranca-de-rede/4.1-tls-no-trafego-externo.md) · ✅ [4.3](../capitulos/04-seguranca-de-rede/4.3-network-policies.md) · ✅ [4.5](../capitulos/04-seguranca-de-rede/4.5-tls-no-trafego-interno.md) |
| Expor aplicações não HTTP (LoadBalancer) e redes adicionais de Pods | [4.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.5-redes-de-pods-e-services.md) | ✅ [5.1](../capitulos/05-redes-nao-http/5.1-servicos-load-balancer.md) · ✅ [5.3](../capitulos/05-redes-nao-http/5.3-redes-adicionais-de-pods.md) |
| Habilitar autosserviço (quotas, limit ranges, template de projeto, self-provisioner) | [6.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.5-reservar-capacidade-de-computacao.md) · [6.7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.7-limitar-capacidade-de-computacao.md) | ✅ [6.1](../capitulos/06-autosservico/6.1-quotas-de-projeto-e-cluster.md) · ✅ [6.3](../capitulos/06-autosservico/6.3-limit-ranges.md) · ✅ [6.5](../capitulos/06-autosservico/6.5-template-de-projeto-e-self-provisioner.md) |
| Gerenciar Operators (OLM, console e CLI) | [1.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/01-introducao-kubernetes-openshift/1.3-componentes-e-edicoes-do-openshift.md) | ✅ [7.1](../capitulos/07-operadores-kubernetes/7.1-operators-e-olm.md) · ✅ [7.3](../capitulos/07-operadores-kubernetes/7.3-instalar-operators-pela-console.md) · ✅ [7.5](../capitulos/07-operadores-kubernetes/7.5-instalar-operators-pela-cli.md) |
| Segurança de aplicações (SCC, service accounts, acesso à API, CronJobs de manutenção) | [3.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/03-containers-e-pods/3.1-criar-containers-e-pods.md) | ✅ [8.1](../capitulos/08-seguranca-aplicacoes/8.1-security-context-constraints.md) · ✅ [8.3](../capitulos/08-seguranca-aplicacoes/8.3-acesso-de-aplicacoes-a-api.md) · ✅ [8.5](../capitulos/08-seguranca-aplicacoes/8.5-manutencao-com-cron-jobs.md) |
| Atualizar o cluster e Operators; APIs obsoletas | [1.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/01-introducao-kubernetes-openshift/1.3-componentes-e-edicoes-do-openshift.md) | ✅ [9.1](../capitulos/09-atualizacoes-openshift/9.1-atualizar-clusters.md) · ✅ [9.3](../capitulos/09-atualizacoes-openshift/9.3-apis-obsoletas.md) · ✅ [9.5](../capitulos/09-atualizacoes-openshift/9.5-atualizar-operators-com-olm.md) |
| Storage, confiabilidade (probes, recursos, HPA) e atualização de aplicações | [Cap. 5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/05-configuracao-e-storage/README.md) · [Cap. 6](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/README.md) · [Cap. 7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/07-atualizacao-aplicacoes/README.md) | — |

## Competências do Capítulo 1

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Gerar, limpar e organizar manifestos (dry-run, `oc explain`, multi-documento) | [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) | Implantar com manifestos |
| Validar (`--dry-run=server`) e comparar (`oc diff`) antes de aplicar | 1.1 | Implantar com segurança; diagnóstico |
| Criar e atualizar com `oc create -f`/`oc apply -f`; entender `last-applied-configuration` | 1.1 | Implantar e atualizar |
| Forçar rollout após mudança de configuração | 1.1 | Atualizar aplicações |
| Aplicar `oc patch` (strategic merge, JSON) | 1.1 | Alterar recursos rapidamente (➕ `--type json`) |
| Estruturar base e overlays; aplicar com `-k` | [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) | Implantar por ambiente |
| Usar `patches`, `labels`, `namespace` e generators | 1.3 | Implantar por ambiente |

## Competências do Capítulo 2

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Avaliar templates (objetos, parâmetros, geradores) no catálogo e em projetos | [2.1](../capitulos/02-aplicacoes-empacotadas/2.1-templates-do-openshift.md) | Implantar aplicações empacotadas |
| Instanciar com `oc new-app --template` e com `oc process \| oc apply` | 2.1 | Implantar aplicações empacotadas |
| Atualizar a partir de template com `--param-file` e `oc diff` | 2.1 | Atualizar aplicações |
| Publicar templates em projetos | 2.1 | Implantar aplicações empacotadas |
| Configurar repositórios e inspecionar charts | [2.3](../capitulos/02-aplicacoes-empacotadas/2.3-charts-do-helm.md) | Implantar aplicações empacotadas |
| Instalar, atualizar, reverter e remover releases com valores por ambiente | 2.3 | Implantar e atualizar aplicações |
| Gerenciar releases pela console (Ecosystem › Helm) | 2.3 | ➕ Uso da console |

## Competências do Capítulo 3

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Configurar o IdP HTPasswd (arquivo, Secret, recurso OAuth) | [3.1](../capitulos/03-autenticacao-autorizacao/3.1-provedor-de-identidade-htpasswd.md) | Gerenciar autenticação |
| Incluir, alterar senha e remover usuários (User e Identity) | 3.1 | Gerenciar autenticação |
| Usar kubeconfig de administrador; remover o `kubeadmin` com segurança | 3.1 | Gerenciar autenticação |
| Conceder `cluster-admin` e outros papéis de cluster | [3.3](../capitulos/03-autenticacao-autorizacao/3.3-permissoes-com-rbac.md) | Gerenciar autorização |
| Criar grupos e conceder papéis de projeto a usuários e grupos | 3.3 | Gerenciar autorização |
| Verificar permissões (`who-can`, `oc auth can-i`) | 3.3 | Diagnóstico (➕ `can-i --as`) |

## Competências do Capítulo 4

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Criar Routes edge, passthrough e re-encrypt com certificados próprios ou da plataforma | [4.1](../capitulos/04-seguranca-de-rede/4.1-tls-no-trafego-externo.md) | Proteger o tráfego externo |
| Gerar certificado assinado (chave, CSR, SAN) e montá-lo como Secret TLS | 4.1 | Proteger o tráfego externo |
| Isolar projetos com default-deny e liberar fluxos por labels de Pods e namespaces | [4.3](../capitulos/04-seguranca-de-rede/4.3-network-policies.md) | Configurar NetworkPolicies |
| Permitir routers e monitoramento; entender o efeito do nó único | 4.3 | Configurar NetworkPolicies |
| Ler AdminNetworkPolicies e sua precedência | 4.3 | ➕ Contexto de administrador |
| Gerar certificados de serviço e injetar a CA em clientes | [4.5](../capitulos/04-seguranca-de-rede/4.5-tls-no-trafego-interno.md) | Proteger o tráfego interno |

## Competências do Capítulo 5

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Expor aplicações não HTTP com Services LoadBalancer e verificar o IP externo | [5.1](../capitulos/05-redes-nao-http/5.1-servicos-load-balancer.md) | Expor aplicações não HTTP/SNI |
| Entender o MetalLB (pool, anúncio L2/BGP) e o esgotamento do pool | 5.1 | Expor aplicações não HTTP/SNI |
| Criar namespaces e UDNs primárias para isolar workloads | [5.3](../capitulos/05-redes-nao-http/5.3-redes-adicionais-de-pods.md) | Redes adicionais de Pods |
| Criar NADs e anexar Pods a redes externas | 5.3 | Redes adicionais de Pods |
| Verificar redes de um Pod pela anotação `network-status` | 5.3 | Diagnóstico |

## Competências do Capítulo 6

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Criar quotas de computação e de contagem de objetos e conferir seu uso | [6.1](../capitulos/06-autosservico/6.1-quotas-de-projeto-e-cluster.md) | Habilitar o autosserviço |
| Limitar vários projetos com ClusterResourceQuota | 6.1 | Habilitar o autosserviço |
| Criar limit ranges com padrões, mínimos e máximos | [6.3](../capitulos/06-autosservico/6.3-limit-ranges.md) | Habilitar o autosserviço |
| Criar e ativar um template de projeto com quota e limit range | [6.5](../capitulos/06-autosservico/6.5-template-de-projeto-e-self-provisioner.md) | Habilitar o autosserviço |
| Restringir ou desligar a criação de projetos (`self-provisioner`) | 6.5 | Habilitar o autosserviço |
| Diagnosticar workloads barrados por quota ou limit range pelos eventos | 6.1 · 6.3 | Diagnóstico |

## Competências do Capítulo 7

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Distinguir cluster operators (CVO) de add-on operators (OLM) e verificar `oc get co` | [7.1](../capitulos/07-operadores-kubernetes/7.1-operators-e-olm.md) | Gerenciar Operators |
| Instalar, inspecionar e desinstalar operators pela web console | [7.3](../capitulos/07-operadores-kubernetes/7.3-instalar-operators-pela-console.md) | Gerenciar Operators |
| Ler PackageManifests e criar OperatorGroup e Subscription pela CLI | [7.5](../capitulos/07-operadores-kubernetes/7.5-instalar-operators-pela-cli.md) | Gerenciar Operators |
| Aprovar install plans manuais e trocar o canal de atualização | 7.5 | Atualizar Operators |
| Usar o operator (CRs, `alm-examples`, `oc explain`) e diagnosticar CSV/InstallPlan | 7.3 · 7.5 | Diagnóstico |

## Competências do Capítulo 8

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Diagnosticar falhas de imagens que exigem root e descobrir a SCC com `scc-subject-review` | [8.1](../capitulos/08-seguranca-aplicacoes/8.1-security-context-constraints.md) | Segurança de aplicações |
| Criar service accounts, conceder SCCs (cluster ou projeto) e associá-las a workloads | 8.1 | Segurança de aplicações |
| Conceder a aplicações acesso à API no próprio projeto, em outro projeto ou no cluster | [8.3](../capitulos/08-seguranca-aplicacoes/8.3-acesso-de-aplicacoes-a-api.md) | Segurança de aplicações |
| Gerar tokens de service account e verificar permissões com `oc auth can-i --as` | 8.3 | Segurança de aplicações |
| Criar Jobs e CronJobs de manutenção com SA, papel e SCC adequados | [8.5](../capitulos/08-seguranca-aplicacoes/8.5-manutencao-com-cron-jobs.md) | Segurança de aplicações / manutenção |

## Competências do Capítulo 9

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Escolher e trocar o canal de atualização; ler `oc adm upgrade` | [9.1](../capitulos/09-atualizacoes-openshift/9.1-atualizar-clusters.md) | Atualizar o cluster |
| Preparar e acompanhar uma atualização (MHC, `Upgradeable`, History) | 9.1 | Atualizar o cluster |
| Identificar APIs obsoletas em uso (`APIRequestCount`, alertas) | [9.3](../capitulos/09-atualizacoes-openshift/9.3-apis-obsoletas.md) | APIs obsoletas |
| Encontrar e aprovar atualizações de operators; trocar canal | [9.5](../capitulos/09-atualizacoes-openshift/9.5-atualizar-operators-com-olm.md) | Atualizar Operators |
| Relacionar compatibilidade de operators e bloqueio do CVO | 9.5 | Atualizar o cluster |

## Estratégia de treino

1. Para cada linha marcada, refaça a tarefa central sem consulta e verifique o resultado.
2. Pratique ler a documentação do produto rapidamente (ela é permitida no exame, segundo a página oficial).
3. Garanta que tudo o que você configurar **persista** após reinicializações.

---
[Certificação](README.md) | [Início](../README.md)
