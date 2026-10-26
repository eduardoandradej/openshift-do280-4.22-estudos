# Glossário

> Cobertura: **Capítulos 1–2**. Termos do DO180 no
> [glossário do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/consulta-rapida/glossario.md).

| Termo | Definição |
|-------|-----------|
| **allowNameChange** | Opção de um patch do Kustomize que permite a um strategic merge patch renomear o recurso |
| **appVersion** | Campo do `Chart.yaml` com a versão da aplicação empacotada (informativo) |
| **Base (Kustomize)** | Diretório com os recursos comuns a todos os ambientes e seu `kustomization.yaml` |
| **behavior (generator)** | `create`, `merge` ou `replace`: como um generator do overlay trata um objeto gerado pela base |
| **Chart (Helm)** | Pacote com `Chart.yaml`, `values.yaml` e `templates/` que descreve uma aplicação |
| **Chart.yaml** | Metadados de um chart: nome, `version`, `appVersion`, descrição |
| **Chave de mesclagem** | Campo que identifica itens de uma lista no strategic merge patch (ex.: `name` em containers, `mountPath` em volumeMounts) |
| **Cluster Samples Operator** | Operator que popula templates e image streams de exemplo no namespace `openshift` |
| **commonLabels** | Campo obsoleto do Kustomize; adiciona labels também a selectors e templates |
| **configMapGenerator** | Gera ConfigMaps a partir de `literals`, `envs` ou `files`, com hash no nome |
| **Declarativo (fluxo)** | Manter o estado desejado em arquivos versionados e aplicá-lo com `oc apply` |
| **Drift** | Diferença entre o que está no Git e o que está no cluster, causada por mudança fora do fluxo |
| **Dry run** | Execução que não persiste nada; `client` (local) ou `server` (validado pelo API server) |
| **generatorOptions** | Opções de todos os generators de uma kustomization (`disableNameSuffixHash`, `labels`, `annotations`) |
| **GitOps** | Prática em que o Git é a fonte da verdade do estado do cluster e ferramentas reconciliam as diferenças |
| **Helm** | Ferramenta que instala, atualiza e reverte aplicações empacotadas como charts |
| **HelmChartRepository / ProjectHelmChartRepository** | Recursos que publicam um repositório de charts na console (cluster inteiro / um projeto) |
| **Hook (Helm)** | Tarefa que o chart executa em momentos do ciclo (antes/depois de instalar ou atualizar) |
| **Imperativo (fluxo)** | Alterar o cluster com uma sequência de comandos de ação |
| **JSON Patch** | Formato (RFC 6902) de operações `op`/`path`/`value` em caminhos exatos |
| **kustomization.yaml** | Arquivo que define recursos, transformações, patches e generators de um diretório Kustomize |
| **Kustomize** | Ferramenta que produz variações de manifestos YAML sem templates; integrada ao `oc` (`-k`, `oc kustomize`) |
| **last-applied-configuration** | Anotação `kubectl.kubernetes.io/last-applied-configuration` com o último manifesto aplicado |
| **Manifesto de recurso** | Arquivo YAML ou JSON que descreve o estado desejado de um ou mais objetos |
| **namePrefix / nameSuffix** | Prefixo/sufixo aplicado pelo Kustomize ao nome de todos os recursos |
| **Overlay** | Diretório Kustomize que referencia uma base e declara as diferenças de um ambiente |
| **Parâmetro (template)** | Lacuna de um template, com valor padrão, obrigatoriedade ou gerador |
| **Processar (template)** | Substituir os parâmetros e obter a lista de manifestos (`oc process`) |
| **Release (Helm)** | Instalação nomeada de um chart em um namespace |
| **Revisão (Helm)** | Cada estado de uma release (install, upgrade, rollback), listado em `helm history` |
| **secretGenerator** | Como o `configMapGenerator`, mas gera Secrets e faz a codificação base64 |
| **Strategic merge patch** | Patch com a estrutura do recurso; mescla listas por chave de mesclagem; padrão do `oc patch` |
| **Template (OpenShift)** | Recurso `template.openshift.io/v1` com objetos parametrizados |
| **Three-way merge** | Comparação feita pelo `oc apply` entre manifesto, objeto vivo e `last-applied-configuration` |
| **values.yaml** | Valores padrão de um chart; sobrescritos por `-f` e `--set` |

---
[Início](../README.md)
