# Glossário

> Cobertura: **Capítulo 1**. Termos do DO180 no
> [glossário do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/consulta-rapida/glossario.md).

| Termo | Definição |
|-------|-----------|
| **allowNameChange** | Opção de um patch do Kustomize que permite a um strategic merge patch renomear o recurso |
| **Base (Kustomize)** | Diretório com os recursos comuns a todos os ambientes e seu `kustomization.yaml` |
| **behavior (generator)** | `create`, `merge` ou `replace`: como um generator do overlay trata um objeto gerado pela base |
| **Chave de mesclagem** | Campo que identifica itens de uma lista no strategic merge patch (ex.: `name` em containers, `mountPath` em volumeMounts) |
| **commonLabels** | Campo obsoleto do Kustomize; adiciona labels também a selectors e templates |
| **configMapGenerator** | Gera ConfigMaps a partir de `literals`, `envs` ou `files`, com hash no nome |
| **Declarativo (fluxo)** | Manter o estado desejado em arquivos versionados e aplicá-lo com `oc apply` |
| **Drift** | Diferença entre o que está no Git e o que está no cluster, causada por mudança fora do fluxo |
| **Dry run** | Execução que não persiste nada; `client` (local) ou `server` (validado pelo API server) |
| **generatorOptions** | Opções de todos os generators de uma kustomization (`disableNameSuffixHash`, `labels`, `annotations`) |
| **GitOps** | Prática em que o Git é a fonte da verdade do estado do cluster e ferramentas reconciliam as diferenças |
| **Imperativo (fluxo)** | Alterar o cluster com uma sequência de comandos de ação |
| **JSON Patch** | Formato (RFC 6902) de operações `op`/`path`/`value` em caminhos exatos |
| **Kustomize** | Ferramenta que produz variações de manifestos YAML sem templates; integrada ao `oc` (`-k`, `oc kustomize`) |
| **kustomization.yaml** | Arquivo que define recursos, transformações, patches e generators de um diretório Kustomize |
| **last-applied-configuration** | Anotação `kubectl.kubernetes.io/last-applied-configuration` com o último manifesto aplicado |
| **Manifesto de recurso** | Arquivo YAML ou JSON que descreve o estado desejado de um ou mais objetos |
| **namePrefix / nameSuffix** | Prefixo/sufixo aplicado pelo Kustomize ao nome de todos os recursos |
| **Overlay** | Diretório Kustomize que referencia uma base e declara as diferenças de um ambiente |
| **secretGenerator** | Como o `configMapGenerator`, mas gera Secrets e faz a codificação base64 |
| **Strategic merge patch** | Patch com a estrutura do recurso; mescla listas por chave de mesclagem; padrão do `oc patch` |
| **Three-way merge** | Comparação feita pelo `oc apply` entre manifesto, objeto vivo e `last-applied-configuration` |

---
[Início](../README.md)
