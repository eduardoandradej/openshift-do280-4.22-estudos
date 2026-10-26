# Roteiro de estudos

## Ordem recomendada

| Etapa | Material | Esforço estimado* |
|-------|----------|-------------------|
| 0 | `00-comecar-aqui` + [revisão do DO180](revisao-do180.md) | 1–2 h |
| 1 | Capítulo 1 — Gerenciamento declarativo | 4–5 h + atividades |
| 2 | Capítulo 2 — Aplicações empacotadas | 4–5 h + atividades |
| 3 | Capítulo 3 — Autenticação e autorização | a definir com o PDF |
| 4 | Capítulo 4 — Segurança de rede | a definir com o PDF |
| 5 | Capítulo 5 — Aplicações não HTTP/SNI | a definir com o PDF |
| 6 | Capítulo 6 — Autosserviço de desenvolvedores | a definir com o PDF |
| 7 | Capítulo 7 — Operators | a definir com o PDF |
| 8 | Capítulo 8 — Segurança de aplicações | a definir com o PDF |
| 9 | Capítulo 9 — Atualizações do OpenShift | a definir com o PDF |
| 10 | Capítulo 10 — Revisão integrada | a definir com o PDF |

\* Estimativa pessoal para orientar o planejamento, não um dado oficial.

## Ritmo sugerido (dia de estudo)

1. **15 min** — "Pré-requisitos e conhecimentos anteriores" e, se preciso, o link do DO180.
2. **25 min** — leitura da lição (Partes A, B, D).
3. **30 min** — Parte C com o terminal aberto (em cluster próprio, quando possível).
4. **Atividade oficial** — somente depois de ler `PRATICA-OFICIAL.md`.
5. **10 min** — Parte F sem olhar as respostas.
6. **5 min** — registro no diário de laboratórios.

## Revisão espaçada

| Quando | O que revisar |
|--------|---------------|
| Dia seguinte | Parte G das lições do dia anterior |
| Fim da semana | `REVISAO.md` dos capítulos da semana |
| Antes do Capítulo 10 | Todos os `RESUMO.md` + `dificuldades-identificadas.md` |
| Preparação EX280 | [certificacao/](../certificacao/README.md) + consulta rápida |

## Treino para o EX280

O exame é prático. Depois de cada capítulo, refaça as tarefas centrais **sem olhar
o material**, cronometrando, e confira o resultado com comandos de verificação
(`oc get`, `oc describe`, `oc auth can-i`, `oc diff`...). A verificação é parte da
tarefa, não um extra.

---
[← Como usar este repositório](como-usar-este-repositorio.md) | [Índice](README.md) | [Próximo: Revisão do DO180 →](revisao-do180.md)
