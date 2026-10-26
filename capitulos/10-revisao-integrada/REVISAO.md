# Revisão integrada — Capítulos 1 a 9

Material **autoral**. Os simulados não reproduzem os labs oficiais nem perguntas de exame;
usam cenários e nomes próprios. Faça-os **em cluster próprio** (OpenShift Local ou um
cluster de testes) com usuário `cluster-admin`, cronometrando.

> ⛔ **Não faça os simulados no ambiente do curso.** O Simulado A altera configurações do
> cluster inteiro (template de projeto, `self-provisioners`), o que deixaria o ambiente
> diferente do esperado pelas atividades oficiais.

---

## Simulado A — Laboratórios de uma universidade (tempo-alvo: 60 min)

### Cenário

Uma universidade usa um cluster para as aulas práticas. Professores criam um projeto por
turma; monitores dão suporte; alunos só usam os projetos das turmas.

### Especificação

1. Crie um provedor HTPasswd (ou use o existente) com os usuários `prof-ana`, `monitor-rui`
   e `aluno-bia` (senhas fictícias de sua escolha).
2. Crie os grupos `professores` (com `prof-ana`), `monitores` (com `monitor-rui`) e
   `turma-a` (vazio, por enquanto).
3. O grupo `monitores` recebe o papel `view` em todo o cluster e uma ClusterRole própria,
   `gerenciar-grupos`, que permite **todas** as operações em `groups`
   (`user.openshift.io`).
4. Apenas `professores` e `monitores` podem criar projetos, e a restrição deve sobreviver a
   reinícios do API server. A mensagem para quem não pode deve ser
   `Peça uma turma à coordenação.`
5. Todo projeto novo deve nascer com:
   - RoleBinding `turma` dando `admin` ao solicitante;
   - label `turma=<nome do projeto>` no namespace;
   - ResourceQuota `turma`: requests até `1` CPU e `1Gi`; limits até `2` CPUs e `2Gi`;
     no máximo `10` Pods;
   - LimitRange `turma` para containers: padrão de limit `300m`/`256Mi`, padrão de request
     `100m`/`128Mi`, máximo `500m`/`512Mi`;
   - NetworkPolicy `turma` que aceita tráfego apenas de Pods do próprio projeto e do router.
6. Como `prof-ana`, crie o projeto `redes-2026`.
7. Como `monitor-rui`, adicione `aluno-bia` ao grupo `turma-a` e dê ao grupo o papel
   `edit` no projeto `redes-2026`.
8. Comprove: `aluno-bia` cria um Deployment em `redes-2026`, mas não cria projetos; o
   Deployment recebe os padrões do limit range; a 11ª réplica não sobe.

### Checklist de verificação

- [ ] `oc get groups` mostra os três grupos com os membros certos.
- [ ] `oc describe clusterrolebinding self-provisioners`: só os dois grupos; anotação `autoupdate: "false"`.
- [ ] `oc new-project x` como `aluno-bia` falha com a mensagem personalizada.
- [ ] `oc get rolebinding,quota,limitrange,networkpolicy -n redes-2026` mostra os quatro objetos `turma`.
- [ ] `oc get ns redes-2026 --show-labels` mostra `turma=redes-2026`.
- [ ] Pod do Deployment de `aluno-bia` com requests/limits padrão; com 11 réplicas, eventos `exceeded quota`.

<details>
<summary>Solução de referência (abra só depois de tentar)</summary>

```bash
# 1–2. Usuários e grupos (HTPasswd conforme a lição 3.1)
oc adm groups new professores prof-ana
oc adm groups new monitores monitor-rui
oc adm groups new turma-a

# 3. Papéis dos monitores
oc adm policy add-cluster-role-to-group view monitores
oc create clusterrole gerenciar-grupos --verb '*' --resource groups.user.openshift.io
oc adm policy add-cluster-role-to-group gerenciar-grupos monitores

# 4. Autosserviço restrito e permanente
oc annotate clusterrolebinding/self-provisioners --overwrite \
  rbac.authorization.kubernetes.io/autoupdate=false
oc patch clusterrolebinding.rbac self-provisioners --type json -p '[{"op":"replace","path":"/subjects","value":[
  {"apiGroup":"rbac.authorization.k8s.io","kind":"Group","name":"professores"},
  {"apiGroup":"rbac.authorization.k8s.io","kind":"Group","name":"monitores"}]}]'

# 5. Testar em rascunho, montar o template, publicar e ativar
oc create namespace rascunho && oc label namespace rascunho turma=rascunho
#   ... criar quota, limit range e policy no rascunho e testar ...
oc adm create-bootstrap-project-template -o yaml > turma-template.yaml
#   ... editar conforme o arquivo abaixo ...
oc create -f turma-template.yaml -n openshift-config
oc patch projects.config.openshift.io cluster --type merge -p \
  '{"spec":{"projectRequestTemplate":{"name":"project-request"},"projectRequestMessage":"Peça uma turma à coordenação."}}'
oc get pod -n openshift-apiserver -w       # aguardar o Pod novo Running

# 6. Projeto
oc login -u prof-ana && oc new-project redes-2026

# 7. Grupo da turma
oc login -u monitor-rui
oc adm groups add-users turma-a aluno-bia
oc adm policy add-role-to-group edit turma-a -n redes-2026

# 8. Verificação
oc login -u aluno-bia
oc new-project x                                         # Forbidden + mensagem
oc create deployment web --image registry.access.redhat.com/ubi9/httpd-24 -n redes-2026
oc get pod -n redes-2026 -o jsonpath='{.items[0].spec.containers[0].resources}{"\n"}'
oc scale deployment web --replicas 11 -n redes-2026 && oc events -n redes-2026 | grep quota
```

`turma-template.yaml` (objetos acrescentados ao template gerado):

```yaml
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: project-request
objects:
- apiVersion: project.openshift.io/v1
  kind: Project
  metadata:
    name: ${PROJECT_NAME}
    labels:
      turma: ${PROJECT_NAME}
    annotations:
      openshift.io/description: ${PROJECT_DESCRIPTION}
      openshift.io/display-name: ${PROJECT_DISPLAYNAME}
      openshift.io/requester: ${PROJECT_REQUESTING_USER}
  spec: {}
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: turma
    namespace: ${PROJECT_NAME}
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: admin
  subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: ${PROJECT_ADMIN_USER}
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: turma
    namespace: ${PROJECT_NAME}
  spec:
    hard:
      requests.cpu: "1"
      requests.memory: 1Gi
      limits.cpu: "2"
      limits.memory: 2Gi
      count/pods: "10"
- apiVersion: v1
  kind: LimitRange
  metadata:
    name: turma
    namespace: ${PROJECT_NAME}
  spec:
    limits:
    - type: Container
      default:
        cpu: 300m
        memory: 256Mi
      defaultRequest:
        cpu: 100m
        memory: 128Mi
      max:
        cpu: 500m
        memory: 512Mi
- apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: turma
    namespace: ${PROJECT_NAME}
  spec:
    podSelector: {}
    policyTypes:
    - Ingress
    ingress:
    - from:                       # item 1: Pods do próprio projeto
      - podSelector: {}
    - from:                       # item 2: router (OU)
      - namespaceSelector:
          matchLabels:
            policy-group.network.openshift.io/ingress: ""
parameters:
- name: PROJECT_NAME
- name: PROJECT_DISPLAYNAME
- name: PROJECT_DESCRIPTION
- name: PROJECT_ADMIN_USER
- name: PROJECT_REQUESTING_USER
```

Observações:

- `podSelector: {}` sozinho em `from` já significa "Pods do mesmo namespace"; o label
  `turma` no namespace serve para identificação e para policies entre projetos.
- O padrão de limit (`300m`) cabe no máximo (`500m`) e é maior que o padrão de request
  (`100m`), respeitando `max ≥ default ≥ defaultRequest`.
- Com 10 Pods no máximo, a 11ª réplica gera `FailedCreate ... exceeded quota`.

</details>

---

## Simulado B — API de notas com TLS, manutenção e pacote (tempo-alvo: 45 min)

### Cenário

A secretaria publica uma API de notas. Ela precisa de TLS de ponta a ponta, isolamento de
rede, um relatório diário e uma variação de produção gerada com Kustomize.

### Especificação

1. Crie o namespace `secretaria` (sem passar pelo template) com o label `area=secretaria`.
2. Gere um certificado autoassinado para `notas.apps.<seu-domínio>` e grave-o no Secret
   TLS `notas-tls`.
3. Implante a API `notas` (imagem de sua escolha que sirva HTTPS na 8443 a partir de
   `/etc/tls/tls.crt` e `/etc/tls/tls.key`), com o Secret montado em `/etc/tls` e probes
   HTTPS na 8443.
4. Exponha com o Service `notas` (443 → 8443) e a Route **passthrough** `notas`.
5. Crie NetworkPolicies: `deny-all` (nega toda entrada) e `notas-router` (só o router, na
   8443).
6. Crie a SA `relatorio-sa` com o papel `view` **apenas** no namespace `secretaria` e um
   CronJob `relatorio` que roda todo dia às 23:30 (UTC), com essa SA, executando
   `oc get pods -n secretaria`.
7. Crie um overlay Kustomize `producao` sobre uma base com o Deployment `notas`, que
   acrescenta o label `ambiente=producao`, define 3 réplicas e requests `100m`/`128Mi`.
8. Comprove: `curl --cacert` funciona pela Route; um Pod de teste em outro namespace não
   alcança o Service; `oc create job --from cronjob/relatorio` lista os Pods; `oc
   kustomize` mostra o label e as 3 réplicas.

### Checklist de verificação

- [ ] `oc get ns secretaria --show-labels` com `area=secretaria`; sem quota/limit range do template.
- [ ] `oc get secret notas-tls -o jsonpath='{.type}'` = `kubernetes.io/tls`.
- [ ] `oc get route notas -o jsonpath='{.spec.tls.termination}'` = `passthrough`.
- [ ] `curl --cacert notas.crt https://notas.apps.<domínio>/` responde.
- [ ] `oc get networkpolicy -n secretaria` mostra as duas policies.
- [ ] `oc auth can-i list pods -n secretaria --as system:serviceaccount:secretaria:relatorio-sa` = `yes`; em outro namespace = `no`.
- [ ] Job manual do CronJob termina `Complete` e o log lista os Pods.

<details>
<summary>Solução de referência (abra só depois de tentar)</summary>

```bash
# 1. Namespace fora do template
oc create namespace secretaria
oc label namespace secretaria area=secretaria

# 2. Certificado e Secret TLS
openssl req -x509 -newkey rsa:2048 -nodes -days 365 \
  -keyout notas.key -out notas.crt \
  -subj "/CN=notas.apps.<domínio>" -addext "subjectAltName=DNS:notas.apps.<domínio>"
oc create secret tls notas-tls --cert notas.crt --key notas.key -n secretaria

# 3–4. Deployment, Service e Route (deployment-notas.yaml abaixo)
oc apply -f deployment-notas.yaml
oc expose deployment/notas --port 443 --target-port 8443 -n secretaria
oc create route passthrough notas --service notas --hostname notas.apps.<domínio> -n secretaria

# 5. Policies (networkpolicies-notas.yaml abaixo)
oc apply -f networkpolicies-notas.yaml

# 6. SA com view só no namespace e CronJob (cronjob-relatorio.yaml abaixo)
oc create sa relatorio-sa -n secretaria
oc adm policy add-role-to-user view -z relatorio-sa -n secretaria
oc apply -f cronjob-relatorio.yaml
oc create job relatorio-manual --from cronjob/relatorio -n secretaria
oc logs job/relatorio-manual -n secretaria

# 7. Overlay
oc kustomize overlays/producao

# 8. Testes
curl --cacert notas.crt https://notas.apps.<domínio>/
oc run teste --image registry.access.redhat.com/ubi9/ubi -n default --rm -it --restart Never -- \
  curl -sk --max-time 5 https://notas.secretaria.svc:443/      # deve expirar
```

`deployment-notas.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: notas
  namespace: secretaria
spec:
  replicas: 1
  selector:
    matchLabels:
      app: notas
  template:
    metadata:
      labels:
        app: notas
    spec:
      containers:
      - name: notas
        image: registry.example.com/secretaria/notas:1.0
        ports:
        - containerPort: 8443
        volumeMounts:
        - name: tls
          mountPath: /etc/tls
          readOnly: true
        readinessProbe:
          httpGet:
            path: /
            port: 8443
            scheme: HTTPS
        livenessProbe:
          httpGet:
            path: /
            port: 8443
            scheme: HTTPS
      volumes:
      - name: tls
        secret:
          secretName: notas-tls
```

`networkpolicies-notas.yaml`:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: secretaria
spec:
  podSelector: {}
  policyTypes:
  - Ingress
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: notas-router
  namespace: secretaria
spec:
  podSelector:
    matchLabels:
      app: notas
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          policy-group.network.openshift.io/ingress: ""
    ports:
    - protocol: TCP
      port: 8443
```

`cronjob-relatorio.yaml`:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: relatorio
  namespace: secretaria
spec:
  schedule: "30 23 * * *"
  timeZone: Etc/UTC
  jobTemplate:
    spec:
      template:
        spec:
          serviceAccountName: relatorio-sa
          restartPolicy: Never
          containers:
          - name: relatorio
            image: registry.redhat.io/openshift4/ose-cli:latest
            command: ["oc", "get", "pods", "-n", "secretaria"]
```

`overlays/producao/kustomization.yaml` (base em `../../base` com o Deployment `notas`):

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
- ../../base
labels:
- pairs:
    ambiente: producao
replicas:
- name: notas
  count: 3
patches:
- target:
    kind: Deployment
    name: notas
  patch: |-
    - op: add
      path: /spec/template/spec/containers/0/resources
      value:
        requests:
          cpu: 100m
          memory: 128Mi
```

Observações:

- `oc create namespace` não aplica o template de projeto (Cap. 6).
- Route passthrough não termina TLS no router: o certificado servido é o do Pod, por isso
  o `curl --cacert notas.crt` valida (Cap. 4).
- A policy do router libera a **porta do Pod** (8443), não a do Service (Cap. 4).
- `add-role-to-user view -z ... -n secretaria` cria um RoleBinding: a SA só lê
  `secretaria` (Cap. 8).
- A imagem do CronJob precisa ter o `oc`; ajuste para uma imagem disponível no seu cluster.

</details>

---

## Questões integradas (sem cronômetro)

1. Um usuário diz que perdeu acesso a todos os projetos depois que você mudou o provedor de identidade. Qual a causa provável e como verificar?
2. Por que o ClusterRoleBinding `self-provisioners` volta ao padrão sozinho, e como evitar?
3. Um projeto criado pelo template tem quota, mas os Pods de um Deployment sem `resources` não sobem. O que falta no template?
4. Uma Route edge funciona; ao trocar para passthrough, ela para de responder. Cite duas causas.
5. Uma NetworkPolicy com `namespaceSelector` e `podSelector` no mesmo item bloqueia algo que deveria passar. Como reescrever para "OU"?
6. Um operator com aprovação manual bloqueia a atualização do cluster por incompatibilidade. Qual a sequência correta?
7. Um CronJob de limpeza falha com `cannot delete resource "namespaces" ... at the cluster scope`. O que conceder?
8. Um chart foi instalado com `--set replicaCount=3`. Um colega roda `helm upgrade` com outro valor e omite o `--set`. O que acontece com `replicaCount`?
9. Um Service `LoadBalancer` fica em `<pending>`. Onde olhar no MetalLB?
10. Por que `oc get apirequestcounts` é parte da preparação de uma atualização de versão menor?

<details>
<summary>Respostas comentadas</summary>

1. As identidades mudaram (novo provedor, novo `identity`), gerando outro usuário ou falha de mapeamento; verifique `oc get users,identities` e o `mappingMethod` do OAuth.
2. A anotação `rbac.authorization.kubernetes.io/autoupdate: "true"` faz o API server restaurá-lo; anote `false` antes de editar.
3. Um LimitRange com `default`/`defaultRequest` (ou exigir `resources` nos workloads).
4. O Pod não serve TLS (certificado não montado/aplicação em HTTP) ou uma NetworkPolicy não libera o router na porta do Pod; também certificado com nome errado.
5. Colocar `namespaceSelector` e `podSelector` em itens separados da lista `from`.
6. Atualizar o operator para uma versão compatível (aprovando o install plan) ou desinstalá-lo; então atualizar o cluster; reinstalar se tiver sido removido.
7. Uma ClusterRole com `delete` (e `get`/`list`) em `namespaces` e um ClusterRoleBinding para a SA do CronJob, referenciada em `serviceAccountName`.
8. Sem `--set`/`-f` nem `--reuse-values`, o Helm usa os valores padrão do chart (o 3 se perde).
9. `oc get ipaddresspool -n metallb-system` (IPs livres) e `oc get svc -A | grep LoadBalancer` (quem ocupa o pool).
10. Para descobrir workloads que usam APIs removidas na versão de destino e corrigi-los antes de atualizar.

</details>

## Autoavaliação final

| Resultado | Ação |
|-----------|------|
| Simulados no tempo e 9–10 questões | Faça os labs oficiais e agende o EX280 |
| Simulados acima do tempo ou 6–8 questões | Revise os capítulos dos itens que travaram e refaça os simulados |
| Menos de 6 questões | Volte aos `RESUMO.md` e às Partes C das lições; registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
