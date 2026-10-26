# Matriz de competências EX280

## Como ler

| Marca | Significado |
|-------|-------------|
| ✅ | Confirmado em fonte oficial lida integralmente (PDF do curso) |
| 🟡 | Confirmado parcialmente (resumo de resultado de busca da página oficial) |
| 🔎 | A confirmar: baseado na estrutura pública conhecida e nos títulos do DO280; conferir na página oficial |
| ➕ | Complementar: ajuda na prática, mas não é objetivo declarado |
| ⏳ | Lição do DO280 ainda não escrita (aguardando PDF) |

## Áreas do exame × material de estudo

| Área do EX280 | Situação | DO180 | DO280 |
|---------------|----------|-------|-------|
| Gerenciar o OpenShift: console, CLI, consultar/formatar/filtrar atributos, logs, eventos, diagnóstico de containers, Pods e cluster | 🟡 | [2.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.1-interfaces-de-linha-de-comando.md) · [2.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.3-inspecionar-recursos-kubernetes.md) · [2.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.5-avaliar-a-saude-do-cluster.md) · [3.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/03-containers-e-pods/3.5-diagnosticar-containers-e-pods.md) | Reforço em todos os capítulos (diagnóstico) |
| Implantar aplicações a partir de manifestos e gerenciá-las de forma declarativa (incluindo Kustomize) | 🔎 | [4.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.1-implantar-a-partir-de-imagem-e-template.md) | ✅ [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) · ✅ [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) |
| Implantar aplicações empacotadas (templates, Helm) | 🔎 | [4.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.1-implantar-a-partir-de-imagem-e-template.md) | ⏳ Cap. 2 |
| Gerenciar autenticação (provedor HTPasswd, usuários, grupos) e autorização (RBAC) | 🔎 | — | ⏳ Cap. 3 |
| Proteger o tráfego de rede (TLS em Routes, NetworkPolicies, TLS interno) | 🔎 | [4.7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.7-escalar-e-expor-aplicacoes.md) | ⏳ Cap. 4 |
| Expor aplicações não HTTP (LoadBalancer) e redes adicionais de Pods | 🔎 | [4.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.5-redes-de-pods-e-services.md) | ⏳ Cap. 5 |
| Habilitar autosserviço (quotas, limit ranges, template de projeto, self-provisioner) | 🔎 | [6.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.5-reservar-capacidade-de-computacao.md) · [6.7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.7-limitar-capacidade-de-computacao.md) | ⏳ Cap. 6 |
| Gerenciar Operators (OLM, console e CLI) | 🔎 | [1.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/01-introducao-kubernetes-openshift/1.3-componentes-e-edicoes-do-openshift.md) | ⏳ Cap. 7 |
| Segurança de aplicações (SCC, service accounts, acesso à API, CronJobs de manutenção) | 🔎 | [3.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/03-containers-e-pods/3.1-criar-containers-e-pods.md) | ⏳ Cap. 8 |
| Atualizar o cluster e Operators; APIs obsoletas | 🔎 | [1.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/01-introducao-kubernetes-openshift/1.3-componentes-e-edicoes-do-openshift.md) | ⏳ Cap. 9 |
| Storage, confiabilidade (probes, recursos, HPA) e atualização de aplicações | 🔎 | [Cap. 5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/05-configuracao-e-storage/README.md) · [Cap. 6](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/README.md) · [Cap. 7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/07-atualizacao-aplicacoes/README.md) | — |

## Competências do Capítulo 1 (✅ confirmadas no PDF)

| Competência | Lição | Relação com o EX280 |
|-------------|-------|---------------------|
| Gerar, limpar e organizar manifestos (dry-run, `oc explain`, multi-documento) | [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) | Implantar com manifestos (🔎) |
| Validar (`--dry-run=server`) e comparar (`oc diff`) antes de aplicar | 1.1 | Implantar com segurança; diagnóstico (🔎) |
| Criar e atualizar com `oc create -f`/`oc apply -f`; entender `last-applied-configuration` | 1.1 | Implantar e atualizar (🔎) |
| Forçar rollout após mudança de configuração | 1.1 | Atualizar aplicações (🔎) |
| Aplicar `oc patch` (strategic merge, JSON) | 1.1 | Alterar recursos rapidamente (🔎; ➕ `--type json`) |
| Estruturar base e overlays; aplicar com `-k` | [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) | Implantar por ambiente (🔎) |
| Usar `patches`, `labels`, `namespace` e generators | 1.3 | Implantar por ambiente (🔎) |

## Estratégia de treino

1. Para cada linha marcada, refaça a tarefa central sem consulta e verifique o resultado.
2. Pratique ler a documentação do produto rapidamente (ela é permitida no exame, segundo a página oficial).
3. Garanta que tudo o que você configurar **persista** após reinicializações.

---
[Certificação](README.md) | [Início](../README.md)
