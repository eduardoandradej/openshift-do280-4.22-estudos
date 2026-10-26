# Estado do projeto

> Registro do estado real **da documentação** para continuar em sessões futuras.
> Atualize ao concluir cada capítulo. As regras permanentes ficam no
> [CLAUDE.md](../CLAUDE.md). O progresso de estudo do aluno fica no
> [checklist geral](checklist-geral.md) e no [diário](diario-de-laboratorios.md).

**Última atualização:** 2026-10-26 (Capítulo 5 concluído)

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
| Capítulo 6 | ⏳ aguardando PDF | — | README estrutural com seções do sumário |
| Capítulo 7 | ⏳ aguardando PDF | — | idem |
| Capítulo 8 | ⏳ aguardando PDF | — | idem |
| Capítulo 9 | ⏳ aguardando PDF | — | idem |
| Capítulo 10 | ⏳ aguardando PDF | — | idem |
| consulta-rapida | 🟡 | — | Cobre os Capítulos 1 a 5 (remete ao DO180 para o restante) |
| exemplos-originais | 🟡 | — | Exemplos dos Capítulos 1 a 5, validados com `kustomize build`, `helm lint`/`helm template` e decodificação estrita (tipos Kubernetes, Route, ANP, UDN/CUDN, NAD, MetalLB) |
| certificacao | 🟡 | — | Matriz EX280 relacionando áreas do exame às lições |

## PDFs recebidos

| Arquivo | Conteúdo | Páginas |
|---------|----------|---------|
| `DO280_4.22_C00_Introducao_p001-014.pdf` | Sumário, convenções, introdução, ambiente, lab | 1–14 |
| `DO280_4.22_C01_Gerenciamento_Declarativo_p015-068.pdf` | Capítulo 1 completo | 15–68 |
| `DO280_4.22_C02_Aplicacoes_Empacotadas_p069-109.pdf` | Capítulo 2 completo | 69–109 |
| `DO280_4.22_C03_Autenticacao_e_Autorizacao_p110-145.pdf` | Capítulo 3 completo | 110–145 |
| `DO280_4.22_C04_Seguranca_de_Rede_p146-198.pdf` | Capítulo 4 completo | 146–198 |
| `DO280_4.22_C05_Aplicacoes_Nao_HTTP_p199-244.pdf` | Capítulo 5 completo | 199–244 |

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
- Sumário completo dos capítulos 2–10 (títulos e tipos de seção) conferido nas p. 3–5.

## Divergências e imprecisões encontradas no PDF (Cap. 1)

| Página | Ponto | Tratamento no repositório |
|--------|-------|---------------------------|
| p. 37 e 40 | Tabela diz que `labels` vale também para selectors; saída mostra label em `matchLabels` | Lição 1.3 ensina o padrão real (`includeSelectors: false`), verificado com kustomize v5.7.1 |
| p. 37 | Diz que `commonLabels` gera erro sugerindo `kustomize edit fix` | No kustomize v5.7.1 é **aviso**; a lição diz que o comportamento pode variar com a versão embutida no `oc` |
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
| p. 113 | kubeconfig do instalador com certificado "que nunca expira" | Lição descreve como certificado de longa duração e credencial de emergência |
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
| p. 149 × p. 178 | Passthrough "mais seguro" × re-encrypt "o mais seguro" | Lição explica que ambos cifram ponta a ponta |
| p. 178 | "Edge é o tipo padrão" | Nota: em manifestos de Route com `spec.tls`, `termination` é obrigatório |
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

## Pendências

| Item | Quando |
|------|--------|
| Desenvolver o Capítulo 6 | Ao receber o PDF do Capítulo 6 |
| Ampliar `consulta-rapida` e `exemplos-originais` | A cada capítulo |
| Assuntos herdados do DO180: SCC, LimitRange/ResourceQuota, criptografia do etcd (RBAC no Cap. 3, TLS em Routes no Cap. 4) | Caps. 6 e 8 |

## Publicação

- Repositório: `eduardoandradej/openshift-do280-4.22-estudos` (já existia com `LICENSE` Apache-2.0; nada foi sobrescrito).
- Visibilidade: definida pelo usuário.
- Fluxo de publicação: por decisão do usuário (2026-10-26), o conteúdo validado vai direto para o branch `main`, sem PR. O primeiro commit foi feito em `claude/gallant-shannon-b4zr78` e integrado à `main` por fast-forward.
- Último commit relevante: ver `git log` da `main` (commit do Capítulo 5).

## Próximo passo

Aguardar o PDF do **Capítulo 6 — Enabling Developer Self-service** (sumário: 6.1 Project
and Cluster Quotas, 6.2 GE, 6.3 Per-project Resource Constraints: Limit Ranges, 6.4 GE,
6.5 The Project Template and the Self-provisioner Role, 6.6 GE, 6.7 Lab, 6.8 Summary).
Ao recebê-lo: ler este arquivo e o `CLAUDE.md`, confirmar páginas e identificadores
`lab`, e seguir o fluxo da seção 6 do `CLAUDE.md`.

---
[Início](../README.md) | [Checklist geral](checklist-geral.md)
