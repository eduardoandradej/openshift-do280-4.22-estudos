# Comandos `oc` por tarefa

> Cobertura: **Capítulo 1**. Os comandos do DO180 estão na
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

## Git nas atividades (apoio)

| Comando | Para quê |
|---------|----------|
| `git clone <url> --branch <b>` | Clonar já na versão desejada |
| `git branch -a` · `git log --oneline` | Ver branches, tags e versões |
| `git checkout <b>` | Trocar de versão |
| `git show` | Ver o que mudou no último commit |

---
[Início](../README.md)
