# Estado do projeto

> Registro do estado real **da documentação** para continuar em sessões futuras.
> Atualize ao concluir cada capítulo. As regras permanentes ficam no
> [CLAUDE.md](../CLAUDE.md). O progresso de estudo do aluno fica no
> [checklist geral](checklist-geral.md) e no [diário](diario-de-laboratorios.md).

**Última atualização:** 2026-10-26 (Capítulo 2 concluído)

## Situação por módulo

| Módulo | Status | PDF recebido | Observações |
|--------|--------|--------------|-------------|
| Estrutura do repositório | ✅ | — | Árvore, modelos, `.gitignore`, navegação, controle de progresso |
| 00-comecar-aqui | ✅ | Cap. 0 (p. 1–14) | Curso, ambiente, comando `lab`, ponte com o DO180 |
| Capítulo 1 | ✅ validado | p. 15–68 | 2 lições + prática, revisão, resumo (sem quiz) |
| Capítulo 2 | ✅ validado | p. 69–109 | 2 lições + prática, revisão, resumo (sem quiz) |
| Capítulo 3 | ⏳ aguardando PDF | — | README estrutural com seções do sumário |
| Capítulo 4 | ⏳ aguardando PDF | — | idem |
| Capítulo 5 | ⏳ aguardando PDF | — | idem |
| Capítulo 6 | ⏳ aguardando PDF | — | idem |
| Capítulo 7 | ⏳ aguardando PDF | — | idem |
| Capítulo 8 | ⏳ aguardando PDF | — | idem |
| Capítulo 9 | ⏳ aguardando PDF | — | idem |
| Capítulo 10 | ⏳ aguardando PDF | — | idem |
| consulta-rapida | 🟡 | — | Cobre os Capítulos 1 e 2 (remete ao DO180 para o restante) |
| exemplos-originais | 🟡 | — | Exemplos dos Capítulos 1 e 2, validados com `kustomize build` e `helm lint`/`helm template` |
| certificacao | 🟡 | — | Matriz EX280 relacionando áreas do exame às lições |

## PDFs recebidos

| Arquivo | Conteúdo | Páginas |
|---------|----------|---------|
| `DO280_4.22_C00_Introducao_p001-014.pdf` | Sumário, convenções, introdução, ambiente, lab | 1–14 |
| `DO280_4.22_C01_Gerenciamento_Declarativo_p015-068.pdf` | Capítulo 1 completo | 15–68 |
| `DO280_4.22_C02_Aplicacoes_Empacotadas_p069-109.pdf` | Capítulo 2 completo | 69–109 |

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
| Desenvolver o Capítulo 3 | Ao receber o PDF do Capítulo 3 |
| Ampliar `consulta-rapida` e `exemplos-originais` | A cada capítulo |
| Assuntos herdados do DO180: SCC, RBAC, LimitRange/ResourceQuota, TLS em Routes, criptografia do etcd | Caps. 3, 4, 6 e 8 |

## Publicação

- Repositório: `eduardoandradej/openshift-do280-4.22-estudos` (já existia com `LICENSE` Apache-2.0; nada foi sobrescrito).
- Visibilidade: definida pelo usuário.
- Fluxo de publicação: por decisão do usuário (2026-10-26), o conteúdo validado vai direto para o branch `main`, sem PR. O primeiro commit foi feito em `claude/gallant-shannon-b4zr78` e integrado à `main` por fast-forward.
- Último commit relevante: ver `git log` da `main` (commit do Capítulo 2).

## Próximo passo

Aguardar o PDF do **Capítulo 3 — Authentication and Authorization** (sumário: 3.1
Configuring the HTPasswd Identity Provider, 3.2 GE, 3.3 Defining and Applying Permissions
with RBAC, 3.4 GE, 3.5 Lab, 3.6 Summary).
Ao recebê-lo: ler este arquivo e o `CLAUDE.md`, confirmar páginas e identificadores
`lab`, e seguir o fluxo da seção 6 do `CLAUDE.md`.

---
[Início](../README.md) | [Checklist geral](checklist-geral.md)
