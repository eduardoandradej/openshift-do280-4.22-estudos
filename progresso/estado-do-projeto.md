# Estado do projeto

> Registro do estado real **da documentação** para continuar em sessões futuras.
> Atualize ao concluir cada capítulo. As regras permanentes ficam no
> [CLAUDE.md](../CLAUDE.md). O progresso de estudo do aluno fica no
> [checklist geral](checklist-geral.md) e no [diário](diario-de-laboratorios.md).

**Última atualização:** 2026-10-26 (Capítulo 10 concluído — documentação do curso completa)

## Situação por módulo

| Módulo | Status | PDF recebido | Observações |
|--------|--------|--------------|-------------|
| Estrutura do repositório | ✅ | — | Árvore, modelos, `.gitignore`, navegação, controle de progresso |
| 00-comecar-aqui | ✅ | Cap. 0 (p. 1–14) | Curso, ambiente, comando `lab`, ponte com o DO180 |
| Capítulo 1 | ✅ validado | p. 15–68 | 2 lições + prática, revisão, resumo (sem quiz) |
| Capítulo 2 | ✅ validado | p. 69–109 | 2 lições + prática, revisão, resumo (sem quiz) |
| Capítulo 3 | ✅ validado | p. 110–145 | 2 lições + prática, revisão, resumo (sem quiz) |
| Capítulo 4 | ✅ validado | p. 146–198 | 3 lições + prática (com armadilhas do ambiente), revisão, resumo (sem quiz) |
| Capítulo 5 | ✅ validado | p. 199–244 | 2 lições + prática (com armadilhas), revisão, resumo (sem quiz) |
| Capítulo 6 | ✅ validado | p. 245–302 | 3 lições + prática (com armadilhas), revisão, resumo (sem quiz) |
| Capítulo 7 | ✅ validado | p. 303–350 | 3 lições + prática (com armadilhas e o quiz 7.2), revisão, resumo |
| Capítulo 8 | ✅ validado | p. 351–393 | 3 lições + prática (com armadilhas), revisão, resumo (sem quiz) |
| Capítulo 9 | ✅ validado | p. 394–433 | 3 lições + roteiro dos 4 quizzes (sem labs), revisão, resumo |
| Capítulo 10 | ✅ validado | p. 434–468 | Lição 10.1 + prática dos 3 labs, revisão com 2 simulados autorais, resumo final do curso |
| consulta-rapida | ✅ | — | Cobre o curso completo (Capítulos 1 a 10); remete ao DO180 para a base |
| exemplos-originais | ✅ | — | Exemplos dos Capítulos 1 a 9 (o Cap. 10 tem os simulados na própria REVISAO), validados com `kustomize build`, `helm lint`/`helm template` e decodificação estrita (tipos Kubernetes, Route, ANP, UDN/CUDN, NAD, MetalLB, quotas, Template, config de projetos, OperatorGroup, Subscription, RBAC de SCC, CronJob, ClusterExtension) |
| certificacao | ✅ | — | Matriz EX280 completa (todas as áreas com lições) e roteiro final de preparação |

## PDFs recebidos

| Arquivo | Conteúdo | Páginas |
|---------|----------|---------|
| `DO280_4.22_C00_Introducao_p001-014.pdf` | Sumário, convenções, introdução, ambiente, lab | 1–14 |
| `DO280_4.22_C01_Gerenciamento_Declarativo_p015-068.pdf` | Capítulo 1 completo | 15–68 |
| `DO280_4.22_C02_Aplicacoes_Empacotadas_p069-109.pdf` | Capítulo 2 completo | 69–109 |
| `DO280_4.22_C03_Autenticacao_e_Autorizacao_p110-145.pdf` | Capítulo 3 completo | 110–145 |
| `DO280_4.22_C04_Seguranca_de_Rede_p146-198.pdf` | Capítulo 4 completo | 146–198 |
| `DO280_4.22_C05_Aplicacoes_Nao_HTTP_p199-244.pdf` | Capítulo 5 completo | 199–244 |
| `DO280_4.22_C06_Autosservico_Desenvolvedores_p245-302.pdf` | Capítulo 6 completo | 245–302 |
| `DO280_4.22_C07_Operadores_Kubernetes_p303-350.pdf` | Capítulo 7 completo | 303–350 |
| `DO280_4.22_C08_Seguranca_Aplicacoes_p351-393.pdf` | Capítulo 8 completo | 351–393 |
| `DO280_4.22_C09_Atualizacoes_OpenShift_p394-433.pdf` | Capítulo 9 completo | 394–433 |
| `DO280_4.22_C10_Revisao_Geral_p434-468.pdf` | Capítulo 10 completo | 434–468 |

Os PDFs **não** são versionados (bloqueados pelo `.gitignore`); o texto foi extraído
apenas no scratchpad da sessão.

## Fatos verificados no PDF

- Material: `DO280-RHOCP4.22-en-1-20260827`, edição 1 (2026-08-27), 468 páginas.
- Numeração: o rodapé "Page NN | 468" coincide com a página do arquivo do capítulo
  somada ao deslocamento (Cap. 1: página 1 do arquivo = p. 15).
- Ambiente: OpenShift 4.22 nó único (`master01`), bare metal UPI, `lab.example.com`;
  máquinas `workstation`, `master01`, `registry` (Quay :8443 + GitLab `git.lab.example.com`),
  `idm` (LDAP), `utility`, `classroom`. Workspace `/home/student/DO280`.
- Comando `lab`: `start`/`finish` em todas as atividades; `grade` só em labs de fim de
  capítulo e comprehensive reviews; `lab status`; logs em `/tmp/log/labs`.
- Capítulo 1: abertura p. 15; 1.1 Resource Manifests (p. 16–25); 1.2 GE
  `declarative-manifests` (p. 26–34); 1.3 Kustomize Overlays (p. 35–46); 1.4 GE
  `declarative-kustomize` (p. 47–57); 1.5 Lab `declarative-review` (p. 58–67, **com**
  `lab grade`; solução a partir da p. 60); 1.6 Summary (p. 68). Sem quiz.
- Capítulo 2: abertura p. 69; 2.1 Using OpenShift Templates (p. 70–75); 2.2 GE
  `packaged-templates` (p. 76–81); 2.3 Deploying Helm Charts (p. 82–93); 2.4 GE
  `packaged-charts` (p. 94–98); 2.5 Lab `packaged-review` (p. 99–108, **com** `lab grade`;
  solução a partir da p. 101); 2.6 Summary (p. 109). Sem quiz.
- Ambiente (Cap. 2): repositório de charts da sala de aula (HTTP interno) e imagens do
  registry interno; menu da console **Ecosystem › Helm**.
- Capítulo 3: abertura p. 110; 3.1 Configuring the HTPasswd Identity Provider
  (p. 111–118); 3.2 GE `auth-providers` (p. 119–128); 3.3 Defining and Applying
  Permissions with RBAC (p. 129–133); 3.4 GE `auth-rbac` (p. 134–137); 3.5 Lab
  `auth-review` (p. 138–144, **com** `lab grade`; solução a partir da p. 140);
  3.6 Summary (p. 145). Sem quiz.
- Ambiente (Cap. 3): o cluster já tem um IdP **LDAP** (usuário administrador do curso
  entra por ele) e grupos vindos do LDAP; kubeconfig e senha do `kubeadmin` ficam na
  máquina `utility`; o `kubeadmin` não deve ser apagado no curso.
- Capítulo 4: abertura p. 146; 4.1 Protecting External Traffic with TLS (p. 147–150);
  4.2 GE `network-ingress` (p. 151–160); 4.3 Configuring Network Policies (p. 161–168);
  4.4 GE `network-policy` (p. 169–176); 4.5 Protecting Internal Traffic with TLS
  (p. 177–180); 4.6 GE `network-svccerts` (p. 181–185); 4.7 Lab `network-review`
  (p. 186–197, **com** `lab grade`; solução a partir da p. 189); 4.8 Summary (p. 198). Sem quiz.
- Ambiente (Cap. 4): cluster de nó único com routers na rede do host (NetworkPolicy não
  bloqueia a Route); `lab start` de `network-ingress`, `network-svccerts` e
  `network-review` já cria o projeto; arquivos de solução em `~/DO280/solutions/`.
- Capítulo 5: abertura p. 199; 5.1 Load Balancer Services (p. 200–206); 5.2 GE
  `non-http-lb` (p. 207–212); 5.3 Configure Additional Pod Networks (p. 213–224); 5.4 GE
  `non-http-multiple` (p. 225–233); 5.5 Lab `non-http-review` (p. 234–243, **com**
  `lab grade`; solução a partir da p. 236); 5.6 Summary (p. 244). Sem quiz.
- Ambiente (Cap. 5): MetalLB em camada 2 com pool de dois IPs no GE 5.2 e um único IP no
  Lab 5.5 (configurado pelo `lab start`); o nó tem uma segunda interface ligada a uma rede
  física acessível pela máquina `utility`; a console tem mais de um provedor de identidade.
- Capítulo 6: abertura p. 245; 6.1 Project and Cluster Quotas (p. 246–255); 6.2 GE
  `selfservice-quotas` (p. 256–262); 6.3 Per-project Resource Constraints: Limit Ranges
  (p. 263–266); 6.4 GE `selfservice-ranges` (p. 267–277, pela console); 6.5 The Project
  Template and the Self-provisioner Role (p. 278–285); 6.6 GE `selfservice-projtemplate`
  (p. 286–293); 6.7 Lab `selfservice-review` (p. 294–301, **com** `lab grade`; solução a
  partir da p. 296); 6.8 Summary (p. 302). Sem quiz.
- Ambiente (Cap. 6): nó único com 6 CPUs de capacidade (cerca de 5,5 alocáveis, segundo a
  saída impressa); o `lab start` do GE 6.6 cria dois usuários e um grupo de provisionadores;
  o Lab 6.7 exige o namespace de rascunho com o nome do enunciado para `lab grade`/limpeza.
- Capítulo 7: abertura p. 303; 7.1 Kubernetes Operators and the Operator Lifecycle
  Manager (p. 304–307); 7.2 Quiz de associação (p. 308; solução p. 309); 7.3 Installing
  Operators by Using the Web Console (p. 310–313); 7.4 GE `operators-web` (p. 314–325);
  7.5 Installing Operators with the CLI (p. 326–332); 7.6 GE `operators-cli`
  (p. 333–340); 7.7 Lab `operators-review` (p. 341–349, **com** `lab grade`; solução a
  partir da p. 343); 7.8 Summary (p. 350).
- Ambiente (Cap. 7): cluster desconectado; o OLM usa um único catálogo espelhado com os
  operators das atividades; na console 4.22 as páginas são **Ecosystem › Software
  Catalog** e **Ecosystem › Installed Operators**.
- Capítulo 8: abertura p. 351; 8.1 Controlling Application Permissions with Security
  Context Constraints (p. 352–355); 8.2 GE `appsec-scc` (p. 356–361); 8.3 Allowing
  Application Access to Kubernetes APIs (p. 362–366); 8.4 GE `appsec-api` (p. 367–371);
  8.5 Cluster and Node Maintenance with Kubernetes Cron Jobs (p. 372–377); 8.6 GE
  `appsec-prune` (p. 378–384); 8.7 Lab `appsec-review` (p. 385–392, **com** `lab grade`;
  solução a partir da p. 387); 8.8 Summary (p. 393). Sem quiz.
- Ambiente (Cap. 8): exercícios alternam `developer` e `admin`; SCCs e papéis de cluster
  são concedidos como `admin`; o GE 8.6 concede `cluster-admin` e `privileged` a uma SA
  só para o exercício.
- Capítulo 9: abertura p. 394; 9.1 Updating Clusters (p. 395–407); 9.2 Quiz (p. 408–409;
  solução p. 410–411); 9.3 Detecting Deprecated Kubernetes API Usage (p. 412–416); 9.4
  Quiz (p. 417; solução p. 418); 9.5 Updating Operators with the OLM (p. 419–424); 9.6
  Quiz (p. 425–426; solução p. 427–428); 9.7 Quiz do capítulo (p. 429–430; solução
  p. 431–432); 9.8 Summary (p. 433). **Sem guided exercises nem lab** (nenhum `lab`).
- Fatos do Cap. 9: OpenShift 4.22 = Kubernetes 1.35; OLM v1 (`ClusterExtension`) é GA na
  4.22, só pela CLI; boot image skew enforcement novo na 4.22 (não se aplica a nó único
  nem a bare metal — caso do cluster do curso).
- Capítulo 10: abertura p. 434; 10.1 Summary of Learning Objectives (p. 435–436); 10.2 Lab
  `compreview-review` (p. 437–445; enunciado p. 437–440, solução a partir da p. 441;
  60 min); 10.3 Lab `compreview-apps` (p. 446–458; enunciado p. 446–449, solução a partir
  da p. 450; 45 min); 10.4 Lab `compreview-package` (p. 459–467; enunciado p. 459–462,
  solução a partir da p. 463; 45 min); 10.5 Summary (p. 468). Todos com `lab grade`.
  Soluções da apostila resumidas (lista de tarefas). O `lab start` do 10.3 restaura o
  template de projeto do 10.2.
- Sumário completo dos capítulos 2–10 (títulos e tipos de seção) conferido nas p. 3–5.

## Divergências e imprecisões encontradas no PDF (Cap. 1)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 37 e 40 | Tabela diz que `labels` vale também para selectors; saída mostra label em `matchLabels` | Lição 1.3 ensina o padrão real (`includeSelectors: false`), verificado com kustomize v5.7.1 |
| p. 23–25 | `--patch-file` "altera" o `mountPath` existente; patch usa container `hello`, mas o objeto vivo mostra `server` | Lição 1.1 explica chave de mesclagem `mountPath` (resultado: dois mounts) e a exigência de nome de container existente; verificado com `strategicpatch` (apimachinery v0.33) |
| p. 31 | Saída de `oc apply` mostra `deployment.apps/exoplanets unchanged` logo após um diff que muda a imagem | Inconsistência de saída sem impacto; não reproduzida |

## Complementos de fonte externa usados no Cap. 1 (marcados 📘)

`--validate=strict|warn|ignore`; códigos de saída do `oc diff`; `oc apply view-last-applied`;
tipos `--type strategic|merge|json` e chaves de mesclagem; server-side apply; aviso de
anotação ausente; omitir `replicas` com HPA; `oc diff -k`; campos de `target` adicionais;
`behavior` de generators; campo `images`; `configurations`/`nameReference`.

Comportamentos testados localmente (kustomize v5.7.1): `labels` × `commonLabels`,
`includeTemplates`, renomeação por JSON Patch e por SMP com/sem `allowNameChange`,
`namePrefix` sem ajuste de `spec.to.name` em Routes, erro de generator sem `behavior`,
`target` usando o nome anterior ao prefixo, `images`.

## Divergências e imprecisões encontradas no PDF (Cap. 2)

Nenhuma imprecisão técnica relevante. Observações: o exemplo de `oc describe template`
(p. 71) mostra um template de exemplo que ainda gera `DeploymentConfig` (API obsoleta
desde 4.14); a lição alerta para preferir templates que geram `Deployment`.

## Complementos de fonte externa usados no Cap. 2 (marcados 📘)

`${{PARAM}}`; `oc process -l`, `--ignore-unknown-parameters`, `--local`; regeneração de
parâmetros gerados a cada processamento; precedência de valores do Helm
(`values.yaml` < `-f` < `--set`, mapas mesclados, listas substituídas); nome e label
`owner=helm` dos Secrets de release (conferido no código do Helm 3.17); `helm template`,
`helm get values|manifest`, `helm status`, `helm show readme`, `helm uninstall`,
`--create-namespace`; rollback criando nova revisão; regra de reaproveitamento de
valores no `helm upgrade` (conferida em `pkg/action/upgrade.go`, Helm 3.17);
`HelmChartRepository`/`ProjectHelmChartRepository` (`helm.openshift.io/v1beta1`).

## Divergências e imprecisões encontradas no PDF (Cap. 3)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 116 | Arquivo criado com `htpasswd -B`, mas a saída de exemplo mostra hash `$apr1$` (MD5) | Lição e resumo indicam que `-B` gera `$2y$` (bcrypt), como nos guided exercises |
| p. 132 | "Por padrão, service accounts não têm papéis" | Lição registra (📘) que algumas service accounts criadas pelo OpenShift recebem vínculos de sistema |
| p. 131 | `system:authenticated:oauth` descrito como o grupo de "todos os usuários autenticados" | Lições 3.1 e 3.3 explicam que ele reúne os autenticados por token OAuth; `system:authenticated` é o de todos |

## Complementos de fonte externa usados no Cap. 3 (marcados 📘)

`oc auth can-i` (`--as`, `--as-group`, `--list`); prevenção de escalada no RBAC; `roleRef`
imutável; `oc create role|rolebinding`; `--role-namespace`; valores de `mappingMethod`
além de `claim`; validade de tokens já emitidos; JSON Patch `add` em
`/spec/identityProviders/-` e risco do merge patch; `oc get co authentication`;
`view` não lê Secrets; grupos `system:authenticated` e `system:authenticated:oauth`;
bindings que citam usuários pelo nome sobrevivem à remoção do User.

Revisão pós-entrega (Cap. 3): validação de tokens contra o User e o UID conferida no
código do `openshift/oauth-apiserver` (`pkg/tokenvalidation/uidvalidator.go` e
`tokenauthenticator.go`); texto sobre `--as`/`--as-group` reescrito para recomendar o
grupo explícito; acrescentados os acessos iniciais de ARO/ROSA/OSD (p. 113) e o aviso
sobre a limpeza de Users/Identities no GE 3.2.

## Divergências e imprecisões encontradas no PDF (Cap. 4)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 164–165 | Item de `from` só com `podSelector` aceitaria Pods "de qualquer namespace" | Seleciona Pods do namespace da política (conferido em `NetworkPolicyPeer`, `k8s.io/api/networking/v1`) |
| p. 167 | Exemplo de ANP com `matchLabels: {sensitive: true}` | Booleano recusado pelo `oc apply`; lição usa `"true"` (verificado com `sigs.k8s.io/network-policy-api` v0.1.5) |
| p. 173 | "Namespaces não recebem labels automaticamente" | Todo namespace tem `kubernetes.io/metadata.name` (a própria saída do GE mostra) |
| p. 151–152 | Texto diz que o `lab start` cria o projeto e o passo 1 manda criá-lo | Inconsistência só do texto: seguindo o PDF, o passo funciona (confirmado pelo usuário); prática oficial explica o que fazer se repetir a atividade |
| p. 158 | Prompt impresso em outro diretório ao criar o Secret TLS | Inconsistência só do texto: na sequência do PDF o comando funciona |
| p. 152 | `oc status` mostra tag de imagem diferente da citada | Inconsistência de saída sem impacto |
| p. 149, 154–156 | Route edge sem `--key/--cert` usaria certificado "da CA interna do Ingress Operator" | É o certificado padrão do ingress controller; no laboratório ele foi substituído e o `issuer` é a CA da sala de aula (revisão pós-entrega) |

Revisão pós-entrega (Cap. 4): certificado padrão do ingress controller (4.1, prática e
resumo); redação neutra sobre "edge é o padrão" (4.5); armadilhas do GE 4.2 (projeto
pode ou não existir) e do Lab 4.7 (criar o Secret TLS antes de aplicar o Deployment);
usos dos exemplos do Cap. 4 no README de `exemplos-originais`.

## Complementos de fonte externa usados no Cap. 4 (marcados 📘)

SAN obrigatório para clientes modernos; `--ca-cert` e `--insecure-policy` em Routes;
`oc extract` da `router-ca`; `--image` no `oc debug`; `policyTypes`; label
`kubernetes.io/metadata.name`; montagem do bundle sobre `/etc/pki/ca-trust/extracted/pem`
substitui as CAs do sistema no container; reinício após rotação de certificado;
`termination` obrigatório em `spec.tls`.

## Regra nova (decisão do usuário)

Ajuste posterior: o usuário confirmou que os exercícios guiados rodam sem erro quando
os comandos do PDF são copiados na ordem. As armadilhas passaram a distinguir
inconsistência do texto × erro fora da sequência (Cap. 4 revisado).


Cada atividade oficial agora tem a seção **Armadilhas do ambiente** no
`PRATICA-OFICIAL.md` (registrada no `CLAUDE.md` e no modelo). Aplicada retroativamente:
Cap. 3, GE 3.4 — `oc auth can-i --as` só funciona como `admin` (usuários comuns recebem
`cannot impersonate`). Os Capítulos 1 e 2 foram revistos e não têm conflitos desse tipo.

## Divergências e imprecisões encontradas no PDF (Cap. 5)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 215 | "IP da sub-rede do nó, uma sub-rede por nó" apresentado para UDNs em geral | É a topologia Layer3; na Layer2 uma sub-rede serve todos os nós (conferido na API `UserDefinedNetwork` do OVN-Kubernetes) |
| p. 217–218 | Manifesto cria a UDN `my-udn`; o comando seguinte consulta `primary-udn` | Inconsistência de exemplo, registrada no resumo |

## Complementos de fonte externa usados no Cap. 5 (marcados 📘)

`BGPPeer`/`BGPAdvertisement`; `--protocol UDP` no `oc expose`; topologias Layer2 × Layer3
conforme a API do OVN-Kubernetes. Comportamentos explicados por dedução técnica e
coerentes com o material: `host-device` exige `strategy: Recreate` (o arquivo do GE já usa);
IP estático + várias réplicas gera conflito.

## Divergências e imprecisões encontradas no PDF (Cap. 6)

Nenhuma imprecisão técnica identificada. Observações sem impacto: hashes de ReplicaSet/Pod
inconsistentes entre saídas impressas do GE 6.2 (irrelevante para a execução).

## Complementos de fonte externa usados no Cap. 6 (marcados 📘)

- Ordem de preenchimento automático do LimitRange (`default ← max`, `defaultRequest ←
  default`, depois `← min`), conferida em `SetDefaults_LimitRangeItem`
  (`kubernetes/kubernetes`, branch `release-1.35`). Detalha a frase da p. 266 e é
  coerente com a saída do GE 6.6 (só `max` → `default` e `defaultRequest` iguais ao max).
- Papéis padrão `admin`/`edit`/`view` apenas **leem** ResourceQuota e LimitRange
  (política RBAC de bootstrap do Kubernetes). As lições recomendam conferir no cluster
  com `oc auth can-i`, pois não foi verificado se o OpenShift agrega permissões extras.
- `oc patch projects.config.openshift.io cluster --type merge` como alternativa ao
  `oc edit`; `spec.scopes`/`scopeSelector` citados como fora do escopo.

## Divergências e imprecisões encontradas no PDF (Cap. 7)

Nenhuma imprecisão técnica identificada. Observações registradas no resumo:

| Página | Ponto | Tratamento |
|--------|-------|------------|
| p. 328 / 330 | O `PackageManifest` do exemplo sugere um namespace e o exemplo de Subscription usa outro | Inconsistência de exemplo; orientar pelo namespace da documentação/`suggested-namespace` |
| p. 350 | Resumo cita "OperatorHub" | Na console 4.22 a página é **Software Catalog** (como o próprio capítulo usa) |
| p. 350 | "OperatorGroup define se o operator é instalado em todo o cluster ou restrito" | Precisão: define os namespaces observados; o workload fica no namespace dele (nota da p. 311) |

## Complementos de fonte externa usados no Cap. 7 (marcados 📘)

- Razões do CSV `NoOperatorGroup`, `TooManyOperatorGroups` e `UnsupportedOperatorGroup` e
  campos de `OperatorGroup`/`Subscription` (`status.installedCSV`, `startingCSV`),
  conferidos em `operator-framework/api` v0.45.0.
- Resolução conjunta das assinaturas de um namespace: uma assinatura `Manual` pode segurar
  atualizações das outras (documentação do OpenShift sobre atualizações manuais).
- Remoção de operators pela CLI (`oc delete subscription` + `oc delete csv`), da
  documentação do OpenShift; a apostila mostra só a console.

## Divergências e imprecisões encontradas no PDF (Cap. 8)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 376 | ConfigMap de exemplo com `app: crictl` direto em `metadata` | Campo inexistente (rejeitado na decodificação estrita contra `k8s.io/api`); o correto é `metadata.labels`, como no GE 8.6 |
| p. 393 | Resumo: "ClusterRoles concedem permissões em todo o cluster" | O escopo depende do binding; ClusterRole por RoleBinding vale só no namespace (a p. 364 diz isso) |
| p. 362 | "O OpenShift não concede permissões à SA `default`" | Sem permissões especiais; as básicas dos grupos `system:serviceaccounts`/`system:authenticated` existem |
| p. 368 | Texto do GE cita a SA sem o sufixo `-sa` | Inconsistência de texto; seguir o nome do comando |

## Complementos de fonte externa usados no Cap. 8 (marcados 📘)

- `oc adm policy add-scc-to-user` cria a ClusterRole `system:openshift:scc:<scc>` e um
  ClusterRoleBinding; com `-n` explícito, cria um RoleBinding no projeto (código do `oc`,
  `pkg/cli/admin/policy/modify_scc.go`, branch `release-4.22`).
- Campos de CronJob/Job (`concurrencyPolicy`, `suspend`, limites de histórico 3/1,
  `backoffLimit` 6) conferidos em `k8s.io/api` v0.37.1; `oc set serviceaccount` e
  `oc create job --from cronjob/` conferidos no código do kubectl.

## Divergências e imprecisões encontradas no PDF (Cap. 9)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 401 | "O CVO atualiza os componentes e depois os que o OLM gerencia" | O CVO atualiza os cluster operators (inclusive o OLM); operators adicionais são atualizados pelo OLM, por Subscription, e devem estar compatíveis antes (p. 405 e seção 9.5) |
| p. 406 | "A *cluster service version* mostra o progresso" | O recurso é o ClusterVersion; CSV é recurso do OLM |
| p. 398 | Mudar de versão menor escolhendo um canal "como `fast-4.22`" | Deve ser o canal da versão de destino |
| p. 413 | "Sem APIs obsoletas em uso, `oc get apirequestcounts` retorna vazio" | 📘 Lista todos os recursos com requisições; obsoletas têm `REMOVEDINRELEASE`; filtro JSONPath da documentação |

## Complementos de fonte externa usados no Cap. 9 (marcados 📘)

- `oc adm upgrade channel <canal>` (conferido no código do `oc`, branch `release-4.22`).
- Política de obsolescência do Kubernetes (beta: 9 meses ou 3 versões, o que for maior).
- Campos da `ClusterExtension` (`upgradeConstraintPolicy`, `namespace`) conferidos em
  `operator-controller` v1.12.0; a lição recomenda `oc explain` por haver diferenças entre
  versões do OLM v1 (ex.: `serviceAccount` obrigatório em versões antigas, ignorado nas
  novas).

## Divergências e imprecisões encontradas no PDF (Cap. 10)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 438, 446, 459 e soluções | Caminhos impressos como `~/DO0019L/labs/...` e `~/DO0019L/solutions/...` | Os demais capítulos usam `~/DO280/...`; a prática orienta conferir com `ls ~` o diretório criado pelo `lab start` (não se afirma erro) |
| p. 434 × p. 468 | Exame chamado "Red Hat Certified System Administrator in OpenShift" na abertura | Nome correto: Red Hat Certified OpenShift Administrator (usado na p. 468) |
| p. 460–461 | Tabela do overlay não distingue labels de anotações | Seguir o `kustomization.yaml` fornecido |

## Validação do Capítulo 10

- [x] Seções 10.1–10.5 representadas, na ordem; identificadores `compreview-review`, `compreview-apps`, `compreview-package`, `lab grade` e tempos conferidos.
- [x] Especificações, valores, senhas e soluções dos labs não reproduzidos; simulados com cenários e nomes próprios.
- [x] YAML dos simulados validado com decodificação estrita (Template com cada objeto, Deployment, NetworkPolicy, CronJob) e `kustomize build` do overlay de exemplo.
- [x] Links relativos, âncoras e links para o DO180 verificados por script; varredura de dados sensíveis sem ocorrências.

## Consolidação final (2026-10-26)

- README, consulta rápida (cobertura 1–10), matriz EX280 (sem pendências, com competências
  de integração), roteiro de preparação em `certificacao/README.md`, checklist, objetivos e
  diário atualizados.
- Nenhum capítulo pendente. O status acima é da **documentação**; o estudo é acompanhado no
  checklist e no diário.

## Validação do Capítulo 9

- [x] Seções 9.1–9.8 representadas, na ordem; quatro quizzes citados sem reproduzir alternativas ou gabaritos.
- [x] Ausência de `lab` e de `lab grade` registrada; avisos ⛔ para comandos de atualização no ambiente do curso.
- [x] `ClusterExtension` validada com decodificação estrita (`operator-controller` v1.12.0); YAML das lições e da consulta rápida conferidos.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 8

- [x] Seções 8.1–8.8 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `appsec-scc`, `appsec-api`, `appsec-prune`, `appsec-review` e `lab grade` (só no 8.7) conferidos.
- [x] Armadilhas no formato revisado; avisos ⛔ para SCCs padrão, `cluster-admin`/`privileged` em SA e limpeza de nós.
- [x] Senhas, hosts, imagens do ambiente e soluções não reproduzidos; exemplos com nomes fictícios.
- [x] ServiceAccount, Deployment, Role/ClusterRole/RoleBinding, ConfigMap e CronJob validados com decodificação estrita (k8s.io/api v0.37.1); YAML das lições e da consulta rápida também; divergência da p. 376 confirmada pelo mesmo verificador.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 7

- [x] Seções 7.1–7.8 representadas, na ordem; quiz 7.2 citado sem reproduzir o gabarito.
- [x] Identificadores `operators-web`, `operators-cli`, `operators-review` e `lab grade` (só no 7.7) conferidos.
- [x] Armadilhas no formato revisado; avisos ⛔ para alterações em arquivos de nós.
- [x] Senhas, nomes de catálogo/versões do ambiente e soluções não reproduzidos; exemplos com nomes fictícios.
- [x] Namespace, OperatorGroup e Subscription validados com decodificação estrita (`operator-framework/api` v0.45.0); blocos YAML das lições e da consulta rápida também.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 6

- [x] Seções 6.1–6.8 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `selfservice-quotas`, `selfservice-ranges`, `selfservice-projtemplate`, `selfservice-review` e `lab grade` (só no 6.7) conferidos.
- [x] Armadilhas do ambiente no formato revisado; avisos ⛔ para template de projeto e `self-provisioners` (cluster inteiro).
- [x] Senhas, usuários de laboratório, soluções e valores oficiais não reproduzidos; exemplos com nomes fictícios.
- [x] ResourceQuota, ClusterResourceQuota, LimitRange (com a ordem `max ≥ default ≥ defaultRequest ≥ min`), Template (cada objeto), Project (config e project), RoleBinding e NetworkPolicy validados com decodificação estrita (`openshift/api`, k8s.io/api v0.37.1); blocos YAML das lições e da consulta rápida também.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 5

- [x] Seções 5.1–5.6 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `non-http-lb`, `non-http-multiple`, `non-http-review` e `lab grade` (só no 5.5) conferidos.
- [x] Armadilhas do ambiente no formato revisado (inconsistência do texto × erro fora da sequência).
- [x] Senhas, IPs, nomes e soluções oficiais não reproduzidos; exemplos usam nomes e faixas fictícios.
- [x] Service, Namespace, UDN, CUDN, NAD (incluindo o JSON do `config`), IPAddressPool, L2Advertisement e Deployment validados com decodificação estrita (`ovn-kubernetes/go-controller`, `network-attachment-definition-client` v1.7.7, `metallb` v0.16.1, k8s.io/api).
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 4

- [x] Seções 4.1–4.8 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `network-ingress`, `network-policy`, `network-svccerts`, `network-review` e `lab grade` (só no 4.7) conferidos.
- [x] Armadilhas do ambiente levantadas para as quatro atividades, sem alterar os passos oficiais.
- [x] Senhas, hostnames, nomes de recursos e soluções oficiais não reproduzidos; exemplos usam nomes fictícios.
- [x] NetworkPolicies, Routes, ANP, Service, ConfigMap, Deployment e patch validados com decodificação estrita (k8s.io/api v0.33, `github.com/openshift/api`, `sigs.k8s.io/network-policy-api` v0.1.5).
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 3

- [x] Seções 3.1–3.6 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `auth-providers`, `auth-rbac`, `auth-review` e `lab grade` (só no 3.5) conferidos.
- [x] Senhas, nomes de Secret/grupos/projetos e soluções oficiais não reproduzidos; exemplos usam nomes e senhas fictícios (`troque-me`).
- [x] Manifestos RBAC validados com decodificação estrita dos tipos `k8s.io/api/rbac/v1`; trecho OAuth validado com parser YAML.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 2

- [x] Seções 2.1–2.6 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `packaged-templates`, `packaged-charts`, `packaged-review` e `lab grade` (só no 2.5) conferidos.
- [x] Enunciados, passos, valores (versões, tags, hosts, senhas) e soluções oficiais não reproduzidos; exemplos usam a aplicação fictícia `catalogo`.
- [x] Chart de exemplo validado com `helm lint` e `helm template` (v3.17); template de exemplo validado com parser YAML.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Validação do Capítulo 1

- [x] Seções 1.1–1.6 representadas, na ordem; ausência de quiz registrada.
- [x] Identificadores `declarative-manifests`, `declarative-kustomize`, `declarative-review` e `lab grade` (só no 1.5) conferidos.
- [x] Enunciados, passos, valores, senhas e soluções oficiais não reproduzidos; exemplos usam a aplicação fictícia `catalogo`.
- [x] YAML dos exemplos validado com parser; `kustomize build` da base e dos dois overlays sem erro.
- [x] Links relativos, âncoras e links para o DO180 verificados por script.
- [x] Varredura de senhas do laboratório, e-mail e marca d'água: nada encontrado.

## Revisão geral das divergências (Caps. 1–5, 2026-10-26)

Pedido do usuário: rever se algo foi "corrigido" sem necessidade. Critério: só é
divergência técnica o que foi **verificado** (teste local, código da API ou a própria
saída do PDF). O resto foi rebaixado para observação e o texto das lições voltou a
seguir a apostila.

| Item | Veredito | Evidência / ação |
|------|----------|------------------|
| `labels` × selectors (Cap. 1, p. 37/40) | Mantida | Testado com kustomize v5.7.1 |
| `commonLabels` "gera erro" (Cap. 1, p. 37) | **Rebaixada** para observação | Não há como confirmar a mensagem do `oc` 4.22; lição volta a seguir a apostila e cita o teste só como nota 📘 |
| Patch de `volumeMounts` (Cap. 1, p. 23–25) | Mantida | Biblioteca `strategicpatch` do Kubernetes |
| Hash `$apr1$` com `-B` (Cap. 3, p. 116) | Mantida (inconsistência de saída) | Os próprios GEs mostram `$2y$` |
| kubeconfig "nunca expira" (Cap. 3, p. 113) | **Retirada** | Não verificada; lição volta a citar a apostila |
| Service accounts "sem papéis" (Cap. 3, p. 132) | Mantida | O próprio GE 3.4 menciona os RoleBindings padrão de service accounts |
| `system:authenticated:oauth` (Cap. 3, p. 131) | Mantida | Código do `oauth-apiserver`: o grupo é adicionado só a tokens OAuth |
| `podSelector` isolado = mesmo namespace (Cap. 4, p. 164–165) | Mantida | Definição de `NetworkPolicyPeer` na API |
| Label booleano em ANP (Cap. 4, p. 167) | Mantida | Decodificação com os tipos da API |
| Labels automáticos de namespace (Cap. 4, p. 173) | Mantida | A saída do GE 4.4 mostra o label |
| "Mais seguro" passthrough × re-encrypt (Cap. 4) | **Rebaixada** para observação | Critérios diferentes nas duas seções; não é erro |
| "Edge é o tipo padrão" (Cap. 4, p. 178) | **Retirada** da lista de divergências | Fica só como complemento 📘 na lição |
| Certificado padrão do ingress (Cap. 4, p. 149/154–156) | Mantida | A saída do GE 4.2 mostra a CA da sala de aula |
| Layer2 × Layer3 de UDN (Cap. 5, p. 215) | Mantida | Comentários da API `UserDefinedNetwork` |
| Saídas/exemplos inconsistentes (p. 31, 152, 217–218) e textos dos GEs 4.2 | Mantidas como inconsistência de texto, sem impacto | Seguindo o PDF, os comandos funcionam (confirmado pelo usuário) |

Cobertura: todos os subtítulos das seções conceituais dos PDFs 1–5 foram levantados por
script e conferidos nas lições; nenhum tópico ficou de fora.

## Pendências

| Item | Quando |
|------|--------|
| Nenhuma pendência de documentação | — |
| Revisões pontuais (erros apontados pelo usuário durante o estudo ou os labs) | Sob demanda |

## Publicação

- Repositório: `eduardoandradej/openshift-do280-4.22-estudos` (já existia com `LICENSE` Apache-2.0; nada foi sobrescrito).
- Visibilidade: definida pelo usuário.
- Fluxo de publicação: por decisão do usuário (2026-10-26), o conteúdo validado vai direto para o branch `main`, sem PR. O primeiro commit foi feito em `claude/gallant-shannon-b4zr78` e integrado à `main` por fast-forward.
- Último commit relevante: ver `git log` da `main` (commit do Capítulo 10 e consolidação final).

## Próximo passo

Documentação concluída. Próximas ações são do **estudo**: seguir o checklist, executar as
atividades oficiais, registrar no diário e usar a matriz EX280 e os simulados do
Capítulo 10. Novas sessões devem tratar apenas correções apontadas pelo usuário, mantendo
as regras do `CLAUDE.md`.

---
[Início](../README.md) | [Checklist geral](checklist-geral.md)
