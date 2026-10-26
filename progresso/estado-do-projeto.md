# Estado do projeto

> Registro do estado real **da documentação** para continuar em sessões futuras.
> Atualize ao concluir cada capítulo. As regras permanentes ficam no
> [CLAUDE.md](../CLAUDE.md). O progresso de estudo do aluno fica no
> [checklist geral](checklist-geral.md) e no [diário](diario-de-laboratorios.md).

**Última atualização:** 2026-10-26 (fundação do repositório + Capítulos 0 e 1)

## Situação por módulo

| Módulo | Status | PDF recebido | Observações |
|--------|--------|--------------|-------------|
| Estrutura do repositório | ✅ | — | Árvore, modelos, `.gitignore`, navegação, controle de progresso |
| 00-comecar-aqui | ✅ | Cap. 0 (p. 1–14) | Curso, ambiente, comando `lab`, ponte com o DO180 |
| Capítulo 1 | ✅ validado | p. 15–68 | 2 lições + prática, revisão, resumo (sem quiz) |
| Capítulo 2 | ⏳ aguardando PDF | — | README estrutural com seções do sumário |
| Capítulo 3 | ⏳ aguardando PDF | — | idem |
| Capítulo 4 | ⏳ aguardando PDF | — | idem |
| Capítulo 5 | ⏳ aguardando PDF | — | idem |
| Capítulo 6 | ⏳ aguardando PDF | — | idem |
| Capítulo 7 | ⏳ aguardando PDF | — | idem |
| Capítulo 8 | ⏳ aguardando PDF | — | idem |
| Capítulo 9 | ⏳ aguardando PDF | — | idem |
| Capítulo 10 | ⏳ aguardando PDF | — | idem |
| consulta-rapida | 🟡 | — | Cobre apenas o Capítulo 1 (remete ao DO180 para o restante) |
| exemplos-originais | 🟡 | — | Exemplos do Capítulo 1, validados com `kustomize build` |
| certificacao | 🟡 | — | Matriz EX280 com itens 🟡/🔎; página oficial não acessível nesta sessão |

## PDFs recebidos

| Arquivo | Conteúdo | Páginas |
|---------|----------|---------|
| `DO280_4.22_C00_Introducao_p001-014.pdf` | Sumário, convenções, introdução, ambiente, lab | 1–14 |
| `DO280_4.22_C01_Gerenciamento_Declarativo_p015-068.pdf` | Capítulo 1 completo | 15–68 |

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
| Conferir a página oficial do EX280 (objetivos e versão) e atualizar `certificacao/` | Quando houver acesso a `redhat.com` |
| Desenvolver o Capítulo 2 | Ao receber o PDF do Capítulo 2 |
| Ampliar `consulta-rapida` e `exemplos-originais` | A cada capítulo |
| Assuntos herdados do DO180: SCC, RBAC, LimitRange/ResourceQuota, TLS em Routes, criptografia do etcd | Caps. 3, 4, 6 e 8 |

## Publicação

- Repositório: `eduardoandradej/openshift-do280-4.22-estudos` (já existia com `LICENSE` Apache-2.0; nada foi sobrescrito).
- Visibilidade: definida pelo usuário.
- Fluxo de publicação: por decisão do usuário (2026-10-26), o conteúdo validado vai direto para o branch `main`, sem PR. O primeiro commit foi feito em `claude/gallant-shannon-b4zr78` e integrado à `main` por fast-forward.
- Último commit relevante: ver `git log` do branch acima (commit do Capítulo 1).

## Próximo passo

Aguardar o PDF do **Capítulo 2 — Deploying Packaged Applications** (sumário: 2.1 Using
OpenShift Templates, 2.2 GE, 2.3 Deploying Helm Charts, 2.4 GE, 2.5 Lab, 2.6 Summary).
Ao recebê-lo: ler este arquivo e o `CLAUDE.md`, confirmar páginas e identificadores
`lab`, e seguir o fluxo da seção 6 do `CLAUDE.md`.

---
[Início](../README.md) | [Checklist geral](checklist-geral.md)
