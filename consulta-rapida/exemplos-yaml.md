# Exemplos YAML comentados

Todos autorais e validados (`yaml.safe_load` e `kustomize build` v5.7.1). Arquivos
aplicáveis em [exemplos-originais/](../exemplos-originais/README.md).
Cobertura: **Capítulo 1**.

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

---
[Início](../README.md)
