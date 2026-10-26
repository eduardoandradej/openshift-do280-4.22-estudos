# Exemplos originais

Manifestos **autorais** para leitura e para testes em clusters próprios (OpenShift
Local, Developer Sandbox) ou apenas para renderização local com `oc kustomize`.
Não são soluções dos exercícios oficiais e não devem ser aplicados no ambiente de
laboratório durante uma atividade.

| Caminho | Conteúdo | Lição |
|---------|----------|-------|
| [manifestos-catalogo/](manifestos-catalogo/) | ConfigMap, Deployment e Service + Route (dois documentos em um arquivo) com `namespace: catalogo-dev` | [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) |
| [patches/patch-limites.yaml](patches/patch-limites.yaml) | Strategic merge patch para `oc patch --patch-file` | 1.1 |
| [kustomize-catalogo/](kustomize-catalogo/) | Base + overlays `desenvolvimento` e `producao` (labels, generator com `behavior: merge`, patch em arquivo e JSON Patch inline) | [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) |
| [template-catalogo/](template-catalogo/) | Template OpenShift com parâmetros (padrão, gerado, `${{...}}`) e arquivo de parâmetros | [2.1](../capitulos/02-aplicacoes-empacotadas/2.1-templates-do-openshift.md) |
| [helm-catalogo/](helm-catalogo/) | Chart Helm mínimo (Deployment, Service, Route opcional, `resources` por valores) + `values-producao.yaml` | [2.3](../capitulos/02-aplicacoes-empacotadas/2.3-charts-do-helm.md) |

O patch fica **fora** de `manifestos-catalogo/` de propósito: `oc apply -f <dir>`
tentaria tratá-lo como manifesto completo e falharia.

## Uso sugerido em cluster próprio

```bash
# Manifestos (1.1)
oc new-project catalogo-dev
oc apply -f exemplos-originais/manifestos-catalogo/ --dry-run=server
oc apply -f exemplos-originais/manifestos-catalogo/
oc diff -f exemplos-originais/manifestos-catalogo/      # sem saída = nada a mudar
oc patch deployment catalogo --patch-file exemplos-originais/patches/patch-limites.yaml
oc delete -f exemplos-originais/manifestos-catalogo/

# Kustomize (1.3) — renderizar não exige cluster
oc kustomize exemplos-originais/kustomize-catalogo/overlays/producao
oc new-project catalogo-prod
oc apply -k exemplos-originais/kustomize-catalogo/overlays/producao
oc get deployment,pods,cm,route -n catalogo-prod

# Template (2.1)
oc process -f exemplos-originais/template-catalogo/template-catalogo.yaml --local --param-file exemplos-originais/template-catalogo/parametros-dev.txt -o yaml
oc process -f exemplos-originais/template-catalogo/template-catalogo.yaml --param-file exemplos-originais/template-catalogo/parametros-dev.txt | oc apply -f - -n catalogo-dev

# Helm (2.3) — lint e template não exigem cluster
helm lint exemplos-originais/helm-catalogo
helm template prod exemplos-originais/helm-catalogo -f exemplos-originais/helm-catalogo/values-producao.yaml
helm install prod exemplos-originais/helm-catalogo -f exemplos-originais/helm-catalogo/values-producao.yaml -n catalogo-prod
helm uninstall prod -n catalogo-prod

oc delete project catalogo-dev catalogo-prod
```

O arquivo de parâmetros do template usa extensão `.txt` porque o `.gitignore` deste
repositório bloqueia arquivos `.env`.

Regras: nenhum Secret real, nenhuma credencial e nenhum host do ambiente oficial.
Todos os arquivos são validados antes do commit: parser YAML, `kustomize build` e `helm lint`/`helm template`.

---
[Início](../README.md)
