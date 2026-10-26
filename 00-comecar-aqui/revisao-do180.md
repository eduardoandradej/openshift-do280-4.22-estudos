# Revisão do DO180: a base que o DO280 pressupõe

O DO280 parte do ponto em que o DO180 termina. Este arquivo **não repete** as lições
do DO180: ele aponta, para cada capítulo do DO280, quais assuntos você precisa
dominar e onde revisá-los no
[guia de estudos do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/README.md).

> Todos os links abaixo foram conferidos no repositório DO180 (branch `main`).
> Para os capítulos do DO280 ainda sem PDF, a coluna "Pré-requisitos" foi montada
> apenas a partir dos **títulos** oficiais das seções (sumário, PDF p. 3–5) e será
> revista quando o capítulo for desenvolvido.

## Autoteste rápido

Se você responde com segurança às perguntas abaixo, a base está boa. Caso contrário,
siga o link.

| Pergunta | Revisar |
|----------|---------|
| Quais são os quatro campos de topo de um objeto e quem escreve o `status`? | [F5 — Kubernetes API e YAML](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/05-kubernetes-api-e-yaml.md) |
| O que um controller faz com o estado desejado e o observado? | [F2 — Arquitetura Kubernetes](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/02-arquitetura-kubernetes.md) |
| Como um Deployment usa `selector` e labels do template? | [F4 — Pods e workloads](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/04-pods-e-workloads.md) |
| Como um Service encontra os Pods e como uma Route aponta para o Service? | [F6 — Redes e serviços](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/06-redes-e-servicos.md) |
| Como descobrir campos com `oc explain` e filtrar com JSONPath? | [2.3 — Inspecionar recursos](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.3-inspecionar-recursos-kubernetes.md) |
| Por que alterar um ConfigMap não muda as variáveis de um Pod em execução? | [5.1 — Externalizar configuração](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/05-configuracao-e-storage/5.1-externalizar-configuracao.md) |
| O que dispara um rollout de Deployment e como reiniciá-lo? | [7.3 — Atualizar imagem e configurações](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/07-atualizacao-aplicacoes/7.3-atualizar-imagem-e-configuracoes.md) |
| Qual a diferença entre request e limit? | [6.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.5-reservar-capacidade-de-computacao.md) e [6.7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.7-limitar-capacidade-de-computacao.md) |

## Mapa por capítulo do DO280

| Capítulo DO280 | Pré-requisitos no DO180 |
|----------------|-------------------------|
| [1 — Gerenciamento declarativo](../capitulos/01-gerenciamento-declarativo/README.md) | [F5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/05-kubernetes-api-e-yaml.md) · [2.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.3-inspecionar-recursos-kubernetes.md) · [3.5 (`oc edit`/`oc patch`)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/03-containers-e-pods/3.5-diagnosticar-containers-e-pods.md) · [4.1 (imperativo × declarativo)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.1-implantar-a-partir-de-imagem-e-template.md) · [5.1](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/05-configuracao-e-storage/5.1-externalizar-configuracao.md) · [7.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/07-atualizacao-aplicacoes/7.3-atualizar-imagem-e-configuracoes.md) |
| [2 — Aplicações empacotadas](../capitulos/02-aplicacoes-empacotadas/README.md) | [4.1 (templates, `oc process`)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.1-implantar-a-partir-de-imagem-e-template.md) · [F5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/05-kubernetes-api-e-yaml.md) |
| [3 — Autenticação e autorização](../capitulos/03-autenticacao-autorizacao/README.md) | [2.1 (login, tokens, contextos)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.1-interfaces-de-linha-de-comando.md) · [F3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/03-arquitetura-openshift.md) · [F8](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/08-ferramentas-de-administracao.md) |
| [4 — Segurança de rede](../capitulos/04-seguranca-de-rede/README.md) | [F6](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/06-redes-e-servicos.md) · [4.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.5-redes-de-pods-e-services.md) · [4.7 (Routes)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.7-escalar-e-expor-aplicacoes.md) · [5.1 (Secrets)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/05-configuracao-e-storage/5.1-externalizar-configuracao.md) |
| [5 — Aplicações não HTTP/SNI](../capitulos/05-redes-nao-http/README.md) | [4.5 (tipos de Service)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.5-redes-de-pods-e-services.md) · [4.7](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.7-escalar-e-expor-aplicacoes.md) |
| [6 — Autosserviço](../capitulos/06-autosservico/README.md) | [6.5 (requests)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.5-reservar-capacidade-de-computacao.md) · [6.7 (limits)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/06-confiabilidade/6.7-limitar-capacidade-de-computacao.md) · [2.1 (projetos)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.1-interfaces-de-linha-de-comando.md) |
| [7 — Operators](../capitulos/07-operadores-kubernetes/README.md) | [F2 (controllers)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/02-arquitetura-kubernetes.md) · [F3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/01-fundamentos-essenciais/03-arquitetura-openshift.md) · [1.3](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/01-introducao-kubernetes-openshift/1.3-componentes-e-edicoes-do-openshift.md) · [2.5 (cluster operators)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.5-avaliar-a-saude-do-cluster.md) |
| [8 — Segurança de aplicações](../capitulos/08-seguranca-aplicacoes/README.md) | [3.1 (UIDs, SCC)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/03-containers-e-pods/3.1-criar-containers-e-pods.md) · [4.3 (Jobs e CronJobs)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/04-deploy-e-redes/4.3-workloads-de-longa-e-curta-duracao.md) · [5.1 (Secrets)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/05-configuracao-e-storage/5.1-externalizar-configuracao.md) |
| [9 — Atualizações do OpenShift](../capitulos/09-atualizacoes-openshift/README.md) | [1.3 (ciclo de vida)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/01-introducao-kubernetes-openshift/1.3-componentes-e-edicoes-do-openshift.md) · [2.5](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.5-avaliar-a-saude-do-cluster.md) · [2.3 (`api-resources`)](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/02-cli-e-apis/2.3-inspecionar-recursos-kubernetes.md) |
| [10 — Revisão integrada](../capitulos/10-revisao-integrada/README.md) | Todo o DO180, em especial a [revisão geral do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/capitulos/08-revisao-geral/README.md) |

## Pendências herdadas do DO180

O próprio DO180 registrou assuntos apenas mencionados e deixados para o DO280:
SCC e RBAC em profundidade, LimitRange e ResourceQuota, TLS em Routes (edge,
passthrough, re-encrypt), criptografia do etcd e RBAC sobre Secrets. Eles serão
tratados nos capítulos correspondentes, quando os PDFs chegarem.

---
[← Roteiro de estudos](roteiro-de-estudos.md) | [Índice](README.md) | [Próximo: Ambiente oficial →](ambiente-oficial.md)
