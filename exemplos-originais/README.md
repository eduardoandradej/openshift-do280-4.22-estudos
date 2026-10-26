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
| [oauth/oauth-htpasswd-trecho.yaml](oauth/oauth-htpasswd-trecho.yaml) | Trecho do recurso OAuth com IdP HTPasswd (referência; não aplicar como está) | [3.1](../capitulos/03-autenticacao-autorizacao/3.1-provedor-de-identidade-htpasswd.md) |
| [rbac/](rbac/) | Role local + RoleBinding; RoleBindings com ClusterRoles padrão para grupo e service account | [3.3](../capitulos/03-autenticacao-autorizacao/3.3-permissoes-com-rbac.md) |
| [routes/](routes/) | Routes edge (com redirecionamento HTTP→HTTPS) e passthrough | [4.1](../capitulos/04-seguranca-de-rede/4.1-tls-no-trafego-externo.md) |
| [network-policies/](network-policies/) | default-deny, allow no mesmo namespace, allow entre projetos (E), routers/monitoramento, AdminNetworkPolicy | [4.3](../capitulos/04-seguranca-de-rede/4.3-network-policies.md) |
| [service-ca/](service-ca/) | Service anotado, ConfigMap de CA, patch de montagem do certificado e Deployment cliente | [4.5](../capitulos/04-seguranca-de-rede/4.5-tls-no-trafego-interno.md) |
| [load-balancer/](load-balancer/) | Service LoadBalancer (RTSP) e referência de pool/anúncio do MetalLB (administrador) | [5.1](../capitulos/05-redes-nao-http/5.1-servicos-load-balancer.md) |
| [redes-adicionais/](redes-adicionais/) | Namespace para UDN primária, UDN, CUDN, NAD host-device e Deployment com anotação | [5.3](../capitulos/05-redes-nao-http/5.3-redes-adicionais-de-pods.md) |
| [autosservico/](autosservico/) | Quotas de computação e de objetos, ClusterResourceQuota, LimitRange com padrões, template de projeto (RoleBinding, LimitRange, ResourceQuota, NetworkPolicy) e configuração global de projetos | [6.1](../capitulos/06-autosservico/6.1-quotas-de-projeto-e-cluster.md) · [6.3](../capitulos/06-autosservico/6.3-limit-ranges.md) · [6.5](../capitulos/06-autosservico/6.5-template-de-projeto-e-self-provisioner.md) |
| [operators/](operators/) | Namespace para operator, OperatorGroup `OwnNamespace`, Subscription com aprovação manual e referência dos outros modos (todos, vários namespaces, selector) | [7.5](../capitulos/07-operadores-kubernetes/7.5-instalar-operators-pela-cli.md) |
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

# RBAC (3.3) — exige o projeto loja e os grupos citados
oc apply -f exemplos-originais/rbac/ --dry-run=server

# Rede (4.x) — exemplos para o projeto loja; a ANP exige administrador do cluster
oc apply -f exemplos-originais/network-policies/01-default-deny.yaml --dry-run=server
oc describe -f exemplos-originais/network-policies/03-allow-entre-projetos.yaml   # após aplicar
oc apply -f exemplos-originais/routes/route-edge.yaml --dry-run=server
oc apply -f exemplos-originais/service-ca/01-service-com-certificado.yaml
oc get secret api-cert -n loja                     # criado pelo service-ca

# Autosserviço (6.x) — quotas e limit range no projeto loja (exige administrador)
oc apply -f exemplos-originais/autosservico/01-quota-computacao.yaml -f exemplos-originais/autosservico/04-limitrange-padroes.yaml
oc describe quota,limitrange -n loja
oc process -f exemplos-originais/autosservico/05-template-projeto.yaml --local -p PROJECT_NAME=teste -p PROJECT_ADMIN_USER=ana -p PROJECT_REQUESTING_USER=ana -o yaml
# ⛔ 03 (cluster quota), 05 publicado em openshift-config e 06 afetam o cluster inteiro: só em cluster próprio

# Operators (7.5) — exige administrador e um catálogo com o pacote; ajuste name/channel/source
oc apply -f exemplos-originais/operators/01-namespace.yaml -f exemplos-originais/operators/02-operatorgroup-ownnamespace.yaml --dry-run=server

oc delete project catalogo-dev catalogo-prod
```

O arquivo de parâmetros do template usa extensão `.txt` porque o `.gitignore` deste
repositório bloqueia arquivos `.env`.

Regras: nenhum Secret real, nenhuma credencial e nenhum host do ambiente oficial.
Todos os arquivos são validados antes do commit: parser YAML, `kustomize build`, `helm lint`/`helm template` e decodificação estrita contra os tipos das APIs (Kubernetes, Route, AdminNetworkPolicy, UDN/CUDN, NAD, MetalLB, ResourceQuota, LimitRange, ClusterResourceQuota, Template, configuração de projetos, OperatorGroup e Subscription (`operator-framework/api`), inclusive cada objeto do template e a ordem `max ≥ default ≥ defaultRequest ≥ min`).

---
[Início](../README.md)
