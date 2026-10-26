# Prática oficial — Capítulo 9

> Ponte para as atividades do curso. **Não** reproduz enunciados, alternativas corretas
> nem gabaritos. Este capítulo **não tem** guided exercises nem lab: todas as atividades são
> **quizzes**, respondidos pela apostila. Nenhum comando `lab` existe para ele.

## Visão geral

| Seção | Tipo | Assunto | Perguntas | Solução | Lição |
|-------|------|---------|-----------|---------|-------|
| 9.2 | Quiz | The Cluster Update Process | PDF p. 408–409 | p. 410–411 | [9.1](9.1-atualizar-clusters.md) |
| 9.4 | Quiz | Detecting Deprecated Kubernetes API Usage | PDF p. 417 | p. 418 | [9.3](9.3-apis-obsoletas.md) |
| 9.6 | Quiz | Updating Operators with the OLM | PDF p. 425–426 | p. 427–428 | [9.5](9.5-atualizar-operators-com-olm.md) |
| 9.7 | Quiz | OpenShift Updates (revisão do capítulo) | PDF p. 429–430 | p. 431–432 | as três |

Páginas verificadas no PDF do Capítulo 9.

> ⛔ **Não atualize o cluster do curso** para "praticar": nenhum passo do curso pede isso.
> Trocar o canal, pausar MachineHealthChecks, executar `oc adm upgrade --to...` ou aprovar
> install plans fora de uma atividade deixa o ambiente diferente do que as atividades
> esperam. Os comandos de leitura (`oc get clusterversion`, `oc adm upgrade` sem
> argumentos, `oc get apirequestcounts`, `oc get csv -A`, `oc get sub -A`) são seguros
> para explorar o ambiente.

## Como usar os quizzes

1. Leia a lição correspondente e responda à **Parte F** dela.
2. Responda ao quiz da apostila **sem** abrir a solução.
3. Confira com a solução e anote os erros em
   [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md).
4. Para cada erro, volte à seção da lição indicada na tabela acima.

---

## 9.2 Quiz: The Cluster Update Process

Canais (`candidate`, `fast`, `stable`, `eus`) e suporte; OTA e salto de versões; papéis do
CVO, MCO, OLM e telemetria; comandos para ver a versão; MachineHealthChecks; rollback;
boot image skew. Base: [9.1](9.1-atualizar-clusters.md).

## 9.4 Quiz: Detecting Deprecated Kubernetes API Usage

Correspondência OpenShift × Kubernetes; maturidade alpha/beta/stable; `oc get
apirequestcounts` e a coluna `REMOVEDINRELEASE`; alertas de APIs a remover. Base:
[9.3](9.3-apis-obsoletas.md).

## 9.6 Quiz: Updating Operators with the OLM

Quem atualiza operators adicionais; atualização automática × manual; onde se aprova;
troca de canal; desinstalação; OLM v1 e `ClusterExtension`. Base:
[9.5](9.5-atualizar-operators-com-olm.md).

## 9.7 Quiz: OpenShift Updates

Revisão de todo o capítulo, incluindo a relação entre atualização de operators e do
cluster. Base: as três lições e a [revisão](REVISAO.md).

## Exploração segura no ambiente (opcional, só leitura)

Durante qualquer atividade de outro capítulo, com o usuário administrador:

```bash
oc get clusterversion
oc adm upgrade                     # o cluster desconectado do curso pode não listar atualizações
oc get co
oc get apirequestcounts | head
oc get csv -A
oc get subscription -A
```

---

**Registro:** anote os quizzes no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 9.5 Atualizar operators com o OLM](9.5-atualizar-operators-com-olm.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
