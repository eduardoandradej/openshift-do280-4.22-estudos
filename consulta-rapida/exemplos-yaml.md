# Exemplos YAML comentados

Todos autorais e validados (`yaml.safe_load`, `kustomize build` v5.7.1, `helm lint`/`helm template` v3.17 e decodificação estrita contra os tipos das APIs: Kubernetes, `route.openshift.io/v1`, `policy.networking.k8s.io/v1alpha1`, `k8s.ovn.org/v1`, `k8s.cni.cncf.io/v1`, `metallb.io/v1beta1`, `quota.openshift.io/v1`, `template.openshift.io/v1`, `config.openshift.io/v1` e `operators.coreos.com`). Arquivos
aplicáveis em [exemplos-originais/](../exemplos-originais/README.md).
Cobertura: **Capítulos 1–7**.

## Vários manifestos em um arquivo

```yaml
apiVersion: v1
kind: Service
metadata:
  name: catalogo
spec:
  selector:
    app: catalogo          # casa com os labels do template do Deployment
  ports:
    - name: http
      port: 8080
      targetPort: 8080
---                        # separa documentos
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: catalogo
spec:
  to:
    kind: Service
    name: catalogo         # nome do Service
  port:
    targetPort: http       # nome da porta do Service
```

## Strategic merge patch (arquivo para `oc patch --patch-file`)

```yaml
spec:
  template:
    spec:
      containers:
        - name: httpd      # chave de mesclagem: precisa existir
          resources:
            limits:
              memory: 256Mi
```

## JSON Patch (para `oc patch --type json` ou `patches` do Kustomize)

```yaml
- op: replace
  path: /spec/replicas
  value: 3
- op: add
  path: /spec/template/spec/containers/0/env/-     # "-" = acrescentar ao fim da lista
  value:
    name: NIVEL_LOG
    value: debug
```

## kustomization.yaml de base

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:                 # arquivos e/ou diretórios com kustomization.yaml
  - deployment.yaml
  - service.yaml
  - route.yaml
configMapGenerator:
  - name: catalogo-config  # nome final recebe sufixo de hash
    literals:
      - NIVEL_LOG=info
```

## kustomization.yaml de overlay

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: catalogo-prod   # o projeto precisa existir
resources:
  - ../../base
labels:
  - pairs:
      ambiente: producao
    # includeSelectors: true  -> também em selectors (cuidado: imutáveis)
    # includeTemplates: true  -> também no template do Pod
patches:
  - path: patch-recursos.yaml          # strategic merge em arquivo
    target: {kind: Deployment, name: catalogo}
  - patch: |-
      - op: replace
        path: /spec/replicas
        value: 3
    target: {kind: Deployment, name: catalogo}
configMapGenerator:
  - name: catalogo-config
    behavior: merge        # obrigatório ao redefinir generator da base
    literals:
      - NIVEL_LOG=warn
```

## Generators: as três fontes e as opções

```yaml
configMapGenerator:
  - name: app-props
    files: [application.properties]   # 1 chave: "application.properties"
  - name: app-env
    envs: [app.env]                   # 1 chave por linha CHAVE=valor
  - name: app-lit
    literals: [MODO=teste]
secretGenerator:
  - name: app-cred
    literals: [SENHA=troque-me]       # valor fictício; codificado em base64
generatorOptions:
  disableNameSuffixHash: true         # nome fixo; sem rollout automático
  labels: {gerado: "sim"}
  annotations: {nota: exemplo}
```

## Template do OpenShift (estrutura)

```yaml
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: catalogo
labels:
  template: catalogo          # vai para todos os objetos gerados
parameters:
  - name: APP_NAME
    value: catalogo           # padrão
    required: true
  - name: REPLICAS
    value: "1"
  - name: ADMIN_PASSWORD
    generate: expression      # gerado se não informado (de novo a cada processamento!)
    from: "[a-zA-Z0-9]{16}"
objects:
  - apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: ${APP_NAME}
    spec:
      replicas: ${{REPLICAS}} # ${{...}}: valor sem aspas (número)
      selector:
        matchLabels: {app: "${APP_NAME}"}
      template:
        metadata:
          labels: {app: "${APP_NAME}"}
        spec:
          containers:
            - name: app
              image: registry.exemplo.com/catalogo:1.0
```

Completo em [template-catalogo.yaml](../exemplos-originais/template-catalogo/template-catalogo.yaml).

## Arquivo de valores do Helm (só o que muda)

```yaml
replicaCount: 3
image:                        # mapas são mesclados com o values.yaml do chart
  repository: registry.exemplo.com:8443/equipe/catalogo
  tag: "2.4"
route:
  host: catalogo.apps.exemplo.com
resources:                    # siga o formato comentado no values.yaml do chart
  requests:
    memory: 128Mi
  limits:
    memory: 256Mi
```

Chart completo em [helm-catalogo/](../exemplos-originais/helm-catalogo/).

## IdP HTPasswd no recurso OAuth (trecho)

```yaml
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
    # ... preserve os IdPs existentes ...
    - name: usuarios-locais        # Identities: usuarios-locais:<usuario>
      mappingMethod: claim
      type: HTPasswd
      htpasswd:
        fileData:
          name: htpasswd-usuarios  # Secret em openshift-config, chave "htpasswd"
```

## Role local + RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: leitor-configmaps
  namespace: loja
rules:
  - apiGroups: [""]                 # grupo core
    resources: ["configmaps"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: suporte-leitor-configmaps
  namespace: loja
subjects:
  - kind: Group                      # também: User, ServiceAccount (com namespace)
    apiGroup: rbac.authorization.k8s.io
    name: suporte
roleRef:                             # imutável
  apiGroup: rbac.authorization.k8s.io
  kind: Role                         # ou ClusterRole (ex.: edit, view)
  name: leitor-configmaps
```

Mais em [exemplos-originais/rbac/](../exemplos-originais/rbac/).

## Routes com TLS

```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: loja-https
spec:
  host: loja-https.apps.exemplo.com
  to:
    kind: Service
    name: loja
  port:
    targetPort: http
  tls:
    termination: edge              # edge | passthrough | reencrypt
    insecureEdgeTerminationPolicy: Redirect
```

Mais em [exemplos-originais/routes/](../exemplos-originais/routes/).

## NetworkPolicies

```yaml
# default-deny: isola todos os Pods do namespace
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
---
# api aceita só Pods app=relatorio de namespaces zona=interna (mesmo item = E)
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: api-aceita-relatorios
spec:
  podSelector:
    matchLabels: {app: api}
  ingress:
    - from:
        - namespaceSelector:
            matchLabels: {zona: interna}
          podSelector:                 # sem "-": E. Com "-": OU
            matchLabels: {app: relatorio}
      ports:
        - port: 8080
          protocol: TCP
```

Routers: `policy-group.network.openshift.io/ingress: ""` · Monitoramento:
`network.openshift.io/policy-group: monitoring`. Mais em
[exemplos-originais/network-policies/](../exemplos-originais/network-policies/).

## service-ca

```yaml
apiVersion: v1
kind: Service
metadata:
  name: api
  annotations:
    service.beta.openshift.io/serving-cert-secret-name: api-cert   # gera Secret TLS
spec:
  selector: {app: api}
  ports:
    - {name: https, port: 443, targetPort: 8443}
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: ca-servicos                    # DEDICADO: o conteúdo é substituído
  annotations:
    service.beta.openshift.io/inject-cabundle: "true"   # chave service-ca.crt
data: {}
```

Mais em [exemplos-originais/service-ca/](../exemplos-originais/service-ca/).

## Service LoadBalancer

```yaml
apiVersion: v1
kind: Service
metadata:
  name: camera-lb
spec:
  type: LoadBalancer          # IP externo vindo da nuvem ou do MetalLB
  selector:
    app: camera
  ports:
    - name: rtsp
      port: 8554
      targetPort: 8554
```

## UDN primária (namespace + UDN)

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: pagamentos-isolado
  labels:
    k8s.ovn.org/primary-user-defined-network: ""   # só na criação
---
apiVersion: k8s.ovn.org/v1
kind: UserDefinedNetwork
metadata:
  name: udn-pagamentos
  namespace: pagamentos-isolado
spec:
  topology: Layer2
  layer2:
    role: Primary
    subnets: ["10.100.0.0/24"]
```

## NAD e anotação no Deployment

```yaml
apiVersion: k8s.cni.cncf.io/v1
kind: NetworkAttachmentDefinition
metadata:
  name: rede-armazenamento
spec:
  config: |-
    {"cniVersion": "0.3.1", "name": "rede-armazenamento", "type": "host-device",
     "device": "eth9", "ipam": {"type": "static", "addresses": [{"address": "192.168.77.10/24"}]}}
```

No Deployment: `spec.template.metadata.annotations: {k8s.v1.cni.cncf.io/networks: rede-armazenamento}`.
Mais em [exemplos-originais/redes-adicionais/](../exemplos-originais/redes-adicionais/) e
[exemplos-originais/load-balancer/](../exemplos-originais/load-balancer/).

## ResourceQuota (computação e contagem)

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: computacao
  namespace: loja
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
    count/deployments.apps: "10"    # outros grupos: count/<recurso>.<grupo>
    count/persistentvolumeclaims: "5"  # grupo core: count/<recurso>
```

## ClusterResourceQuota

```yaml
apiVersion: quota.openshift.io/v1
kind: ClusterResourceQuota
metadata:
  name: equipe-pagamentos
spec:
  quota:            # hard fica dentro de spec.quota
    hard:
      requests.cpu: "10"
  selector:
    labels:
      matchLabels:
        equipe: pagamentos
```

## LimitRange com padrões

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: padroes
  namespace: loja
spec:
  limits:
  - type: Container
    default: {cpu: 500m, memory: 512Mi}         # limit injetado
    defaultRequest: {cpu: 250m, memory: 256Mi}  # request injetado
    max: {cpu: "1", memory: 1Gi}
    min: {cpu: 100m, memory: 128Mi}
```

## Template de projeto e configuração global

Template em `openshift-config` (parta de `oc adm create-bootstrap-project-template -o yaml`
e acrescente itens a `objects:` com `namespace: ${PROJECT_NAME}`). Ativação:

```yaml
apiVersion: config.openshift.io/v1
kind: Project
metadata:
  name: cluster
spec:
  projectRequestTemplate:
    name: project-request
  projectRequestMessage: "Peça projetos pelo chamado na fila Plataforma."
```

Exemplo completo em [exemplos-originais/autosservico/](../exemplos-originais/autosservico/).

## OperatorGroup e Subscription

```yaml
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: auditoria-operator
  namespace: auditoria-operator
spec:
  targetNamespaces:            # OwnNamespace; spec: {} = todos os namespaces
  - auditoria-operator
---
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: auditoria-operator
  namespace: auditoria-operator
spec:
  name: auditoria-operator     # Package Name do PackageManifest
  channel: stable
  source: catalogo-interno     # Catalog Source
  sourceNamespace: openshift-marketplace
  installPlanApproval: Manual  # aprovar: oc patch installplan ... approved=true
```

Mais em [exemplos-originais/operators/](../exemplos-originais/operators/).

---
[Início](../README.md)
