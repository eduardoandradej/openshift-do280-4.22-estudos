# CLAUDE.md — Regras permanentes do projeto

Este arquivo orienta qualquer sessão futura do Claude Code neste repositório.
Leia-o inteiro antes de criar ou alterar conteúdo. Em seguida leia
[progresso/estado-do-projeto.md](progresso/estado-do-projeto.md) e o README do
último capítulo concluído. **Não dependa da memória da conversa.**

## 1. Propósito

Guia pessoal de estudos do curso **Red Hat OpenShift Administration II: Configuring
a Production Cluster (DO280), versão 4.22** (OpenShift Container Platform 4.22).
Objetivo final: competência administrativa real e preparação para o exame **EX280**.

O repositório **não é uma tradução** da apostila. É material autoral: explicações
próprias, exemplos originais, diagnósticos, revisões e ponte para os laboratórios
oficiais, que são executados **somente** no ambiente da Red Hat.

## 2. Repositório de referência (DO180)

- Referência metodológica obrigatória: <https://github.com/eduardoandradej/openshift-do180-4.22-estudos>
  (branch `main`). Modelos, estrutura de capítulos, `PRATICA-OFICIAL.md`,
  `REVISAO.md`, `RESUMO.md`, consulta rápida e controle de progresso foram
  herdados dele e adaptados.
- **Nunca** alterar o DO180 (arquivos, branches, commits, configurações). Se for
  preciso consultá-lo, clonar em diretório temporário fora deste projeto (scratchpad).
- Links para o DO180 usam URLs absolutas no formato
  `https://github.com/eduardoandradej/openshift-do180-4.22-estudos/blob/main/<caminho>`
  e **somente caminhos verificados** no clone de consulta. Não inventar arquivos.
- Não copiar lições do DO180: retomar o assunto em poucas linhas na seção
  **Pré-requisitos e conhecimentos anteriores** e apontar o link.

## 3. Regras de conteúdo (obrigatórias)

- Texto didático em **português brasileiro**; nomes técnicos, comandos, campos YAML,
  recursos, APIs e objetos Kubernetes/OpenShift permanecem em **inglês**.
- Respeitar a **cronologia e a numeração oficial** de capítulos e seções.
- **Não** traduzir capítulos ou exercícios integralmente; **não** copiar parágrafos
  extensos, figuras, tabelas proprietárias, enunciados, passos, soluções ou gabaritos
  (inclusive respostas de quizzes oficiais).
- **Não** versionar PDFs, extrações de texto, credenciais, tokens, kubeconfigs,
  dados da assinatura, nomes de usuário/e-mail da marca d'água do PDF nem as
  senhas do ambiente de laboratório (nem as literais usadas nos manifestos das atividades).
- Referências a páginas somente após verificação, no formato `PDF p. NN`
  (numeração impressa no rodapé do PDF completo, que coincide com a do arquivo do capítulo).
- Identificadores de `lab` somente se verificados no PDF. Nunca inventar.
- `lab grade` apenas quando a atividade oficial aceitar avaliação.
- Conteúdo vindo da documentação pública ou de testes próprios é marcado com
  📘 **Fonte externa** (ou "verificado com kustomize vX" quando testado localmente).
- Não presumir que comportamentos de versões anteriores ou posteriores valem para 4.22.
- Quando o PDF tiver imprecisão técnica, escrever a forma correta e registrar a
  divergência em `progresso/estado-do-projeto.md`.
- Regra central: **simplificar a explicação, nunca simplificar incorretamente a tecnologia.**

## 4. Formato das lições

Toda lição conceitual segue [modelos/modelo-licao.md](modelos/modelo-licao.md):
Identificação → **Pré-requisitos e conhecimentos anteriores** (com links DO180) →
Partes A (conceito), B (modelo mental e arquitetura), C (aplicação prática, com a
subseção **Raciocínio operacional e diagnóstico**), D (relações), E (preparação para
o lab), F (fixação com respostas recolhíveis), G (essencial).
Cada lição termina com a barra `Anterior | Índice do capítulo | Próximo`.

Comandos importantes na Parte C informam: finalidade, sintaxe, parâmetros,
permissões, objetos afetados, comportamento esperado, verificação e erros.
Sempre distinguir **somente leitura** 🔍 de **altera o cluster** ✏️.

Cada capítulo em `capitulos/NN-nome/` contém:
`README.md` (índice), um arquivo por seção conceitual (`N.M-titulo.md`),
`PRATICA-OFICIAL.md`, `REVISAO.md`, `RESUMO.md`.

## 5. Proibições operacionais

- Não executar comandos `lab` nem exercícios oficiais; não criar simuladores,
  ambientes paralelos, scripts de infraestrutura nem avaliadores próprios.
- Exemplos de Secret ficam com valores fictícios e evidentes (`troque-me`).
- **Visibilidade:** o repositório deve ser **privado**. Em 2026-10-26 ele estava
  público; o usuário decidiu publicar primeiro e torná-lo privado depois. Por isso
  todo conteúdo deve ser seguro para leitura pública.
- Não publicar material restrito da assinatura Red Hat.

## 6. Fluxo de trabalho por capítulo (metodologia incremental)

1. O usuário envia o PDF **de um capítulo**. Extrair texto apenas no scratchpad,
   nunca dentro do repositório.
2. Confirmar capítulo, numeração, limites de páginas, seções, atividades e `lab`.
3. Produzir as lições seção por seção; depois PRATICA-OFICIAL, REVISAO, RESUMO.
4. Atualizar `consulta-rapida/` **apenas** com o que o capítulo cobriu.
5. Controle de qualidade: cobertura, ordem, precisão de comandos, YAML válido
   (e `kustomize build` dos exemplos), links e âncoras verificados por script,
   varredura de senhas/e-mail/marca d'água, português revisado, sem transcrição.
6. Atualizar `progresso/estado-do-projeto.md`, `progresso/checklist-geral.md`,
   `progresso/objetivos-de-aprendizagem.md`, `certificacao/` e o `README.md`.
7. Commit organizado por capítulo e push direto no branch `main` (decisão do
   usuário em 2026-10-26: material de estudo, sem PR).
8. Relatar o resultado e **parar**. Não avançar para o capítulo seguinte sem o PDF.

Capítulos sem PDF ficam com README mínimo marcado `⏳ Aguardando PDF do capítulo`.
Nunca marcar como concluído o que não foi desenvolvido e validado.

**Documentação concluída ≠ estudo concluído.** O status em `estado-do-projeto.md`
é da documentação; o `checklist-geral.md` e o diário são do aluno.

## 7. Ambiente de laboratório (fatos do Cap. 0)

Cluster OpenShift 4.22 de nó único (`master01`), bare metal UPI, domínio
`lab.example.com`, registry Quay + GitLab em `registry`, LDAP em `idm`.
Workspace do curso: `/home/student/DO280`. Detalhes em
[00-comecar-aqui/ambiente-oficial.md](00-comecar-aqui/ambiente-oficial.md).

## 8. Estado do projeto

O estado efetivo (capítulos, fatos verificados, divergências, pendências e próximo
passo) fica em **[progresso/estado-do-projeto.md](progresso/estado-do-projeto.md)**.
Atualize-o ao final de cada capítulo.
