# Preparação para os laboratórios

| Campo | Valor |
|-------|-------|
| Referência | Capítulo 0 — *Performing Lab Exercises*, PDF p. 12–14 |

## Tipos de atividade

| Tipo | Como funciona | `lab grade`? |
|------|---------------|--------------|
| **Quiz / Matching** | Verificação de conhecimento quando a prática não é viável | Não |
| **Guided Exercise** | Prática passo a passo logo após uma seção conceitual | Não |
| **Lab de fim de capítulo** | Passos de alto nível baseados nos guided exercises, sem cada comando; há solução comentada | Sim |
| **Comprehensive Review** (Cap. 10) | Uma **especificação** do resultado, sem os passos; pode cobrir o curso inteiro | Sim |

Regra do curso: **todas** as atividades aceitam `start` e `finish`; **somente**
labs de fim de capítulo e comprehensive reviews aceitam `grade`. Cada
`PRATICA-OFICIAL.md` registra o que foi verificado no PDF do capítulo.

## O comando `lab`

Formato: `lab <ação> <atividade>`, no `workstation`, como `student`.

| Ação | O que faz |
|------|-----------|
| `start` | Verifica e prepara recursos: ajusta configurações, cria recursos, confirma serviços e resultados exigidos. Permite fazer qualquer atividade **sem ter feito as anteriores** |
| `grade` | Avalia o trabalho e lista critérios com `PASS`/`FAIL`. Corrija os `FAIL` e rode de novo |
| `finish` | Limpa os recursos da atividade; ela pode ser repetida quantas vezes quiser |
| `status` | Mostra o estado das atividades (`lab status`) |

Regras importantes:

- **Uma atividade ativa por vez.** Se outra estiver em andamento, o `lab` pede para finalizá-la.
- **Tab completion:** `lab start` + Tab duas vezes lista as atividades.
- Cada `lab start` cria um **projeto OpenShift específico** da atividade; o `lab finish` o remove.
- O comando roda scripts Python que montam o diretório de cada atividade.

## Diretórios e logs

| Caminho | Conteúdo |
|---------|----------|
| `/home/student/DO280` | Workspace do curso |
| `/home/student/DO280/labs/<atividade>` | Arquivos copiados pelo `lab start` daquela atividade |
| `/tmp/log/labs/` | Um log por atividade; consulte quando `start`/`finish` falhar |
| `/home/student/.grading/config.yaml` | Configuração do curso usada pelo `lab`; **não altere** |

## Problemas conhecidos de rotina

| Sintoma | Causa | O que fazer |
|---------|-------|-------------|
| `lab start` falha ao verificar a API ou credenciais logo após criar o ambiente | O cluster pode levar **até 15 minutos** para ficar disponível | `lab finish <atividade>`, aguardar, `lab start` de novo |
| `lab start` falha ao repetir uma atividade | O projeto anterior ainda está sendo removido (até **10 minutos**) | Aguardar e tentar novamente |
| `lab start` recusa iniciar | Outra atividade está ativa | `lab finish` da atividade ativa |
| `oc` responde `Unauthorized` | Sessão expirada | Refazer `oc login` |
| Recurso "não existe" | Projeto errado | `oc project`; usar `-n` |
| `git clone` traz a versão errada | Branch não informada | `git branch -a`; `git checkout <branch>` |

## Checklist de execução

**Antes**
- [ ] Li as lições da seção e o `PRATICA-OFICIAL.md` do capítulo.
- [ ] Sei explicar cada objeto que a atividade vai criar ou alterar.
- [ ] `lab start <atividade>` terminou sem erro.

**Durante**
- [ ] Leio a saída de cada comando antes de seguir.
- [ ] Antes de alterar: `--dry-run=server` e/ou `oc diff`.
- [ ] Depois de alterar: verifico (`oc get`, `oc describe`, console, navegador).
- [ ] Se travar, volto à Parte C da lição antes de abrir a solução.

**Depois**
- [ ] `lab grade <atividade>` com tudo `PASS` (quando aplicável).
- [ ] `lab finish <atividade>` executado.
- [ ] Registro no [diário de laboratórios](../progresso/diario-de-laboratorios.md).
- [ ] Dificuldades em [dificuldades-identificadas.md](../progresso/dificuldades-identificadas.md).

---
[← Ambiente oficial](ambiente-oficial.md) | [Índice](README.md) | [Próximo: Capítulo 1 →](../capitulos/01-gerenciamento-declarativo/README.md)
