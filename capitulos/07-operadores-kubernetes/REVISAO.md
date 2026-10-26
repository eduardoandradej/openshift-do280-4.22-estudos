# Revisão — Capítulo 7

Questões **autorais**. Elas não reproduzem atividades oficiais, o quiz da seção 7.2 nem
perguntas de exame. Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Explique o padrão operator usando um exemplo próprio (que não seja banco de dados).
2. Compare cluster operators e add-on operators: quem instala, quem atualiza e como ver o estado.
3. Qual a diferença entre CatalogSource e PackageManifest?
4. Qual a diferença entre o modo de instalação e o namespace instalado de um operator?
5. Para que servem o OperatorGroup e a Subscription? Quais outros recursos o OLM cria sozinho?
6. O que muda entre `installPlanApproval: Automatic` e `Manual`?
7. Por que o OLM e a console exibem um CSV em vários namespaces para alguns operators?
8. Que dois conjuntos de workloads um operator produz, e por que isso importa no diagnóstico?

## Parte 2 — Comandos (escreva o comando)

9. Listar os cluster operators e ver as condições completas do operator `ingress`.
10. Listar os catálogos disponíveis no cluster.
11. Mostrar o canal padrão do pacote `cofre-operator`.
12. Listar os install plans do namespace `cofre` e aprovar o plano `install-abc12`.
13. Trocar a Subscription `cofre-operator` do namespace `cofre` para o canal `stable-2`.
14. Listar as CRDs que o CSV `cofre-operator.v2.1.0` possui.
15. Ver os exemplos de CR embutidos no mesmo CSV.
16. Remover pela CLI o operator assinado por `cofre-operator` no namespace `cofre`.

## Parte 3 — Diagnóstico

17. A Subscription existe há 10 minutos e não há CSV. `oc describe operator` mostra `InstallPlanPending`. Causa e ação.
18. O CSV mostra a razão `TooManyOperatorGroups`. O que aconteceu?
19. O CSV está `Succeeded`, mas CRs criados no projeto `app` não produzem nada. O operator foi instalado com `targetNamespaces: [cofre]`. Explique.
20. `oc create -f cr.yaml` responde que não existe o tipo do recurso. Qual a hipótese mais provável?
21. Um operator com aprovação automática não atualizou, mas outro operator no mesmo namespace usa aprovação manual. O que verificar?
22. `oc get co` mostra `DEGRADED True` para `authentication`. Por onde começar?

---

<details>
<summary>Respostas comentadas</summary>

1. Exemplo: um operator de cache define o tipo `Cache`; ao criar um `Cache` com tamanho e réplicas, o operator cria StatefulSet, Service e PDB, e ajusta tudo quando o CR muda.
2. Cluster operators: instalados e atualizados pelo CVO junto com o OpenShift; estado em `oc get clusteroperator`. Add-on: instalados e atualizados pelo OLM, com ciclo próprio; estado em CSV/`Operator` e na página **Installed Operators**.
3. CatalogSource aponta o repositório (imagem de catálogo); PackageManifest é a entrada de um operator específico, com canais, versões e modos.
4. O modo diz em quais namespaces o operator observa CRs; o namespace instalado é onde o workload do operator roda.
5. OperatorGroup define o escopo observado; Subscription pede pacote, catálogo, canal e aprovação. O OLM cria InstallPlan, CSV, `Operator`, workload e CRDs.
6. `Automatic`: atualizações do canal aplicadas assim que surgem. `Manual`: o install plan fica com `approved: false` até o administrador aprovar.
7. Operators que observam vários namespaces têm o CSV copiado para cada namespace observado.
8. O workload do operator (OLM) e os workloads dos CRs (operator). Falha de instalação se investiga em CSV/InstallPlan; falha de funcionamento, nos workloads, Pods, logs e eventos.
9. `oc get clusteroperator` e `oc describe clusteroperator ingress`
10. `oc get catalogsource -n openshift-marketplace`
11. `oc get packagemanifest cofre-operator -o jsonpath='{.status.defaultChannel}{"\n"}'` (ou `oc describe packagemanifest cofre-operator`)
12. `oc get installplan -n cofre` e `oc patch installplan install-abc12 -n cofre --type merge -p '{"spec":{"approved":true}}'`
13. `oc patch subscription cofre-operator -n cofre --type merge -p '{"spec":{"channel":"stable-2"}}'`
14. `oc get csv cofre-operator.v2.1.0 -n cofre -o jsonpath='{.spec.customresourcedefinitions.owned[*].name}{"\n"}'`
15. `oc get csv cofre-operator.v2.1.0 -n cofre -o jsonpath='{.metadata.annotations.alm-examples}'`
16. `oc delete subscription cofre-operator -n cofre` e `oc delete csv <CSV instalado> -n cofre` (obtido em `status.installedCSV` da Subscription); depois, a limpeza indicada pela documentação.
17. Aprovação manual: o plano espera aprovação. Aprove com `oc patch installplan ... approved=true`.
18. Existe mais de um OperatorGroup no namespace; deixe só um.
19. O operator só observa `cofre`; CRs em `app` são ignorados. Crie os CRs em `cofre` ou reinstale com o escopo adequado.
20. A CRD não existe: o operator não está instalado (ou o CSV ainda não terminou).
21. Se há install plan pendente no namespace: o OLM resolve as assinaturas de um namespace em conjunto, e a política manual de uma pode segurar a atualização da outra.
22. `oc describe clusteroperator authentication` (mensagem das condições e `relatedObjects`) e os Pods dos namespaces relacionados.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 8 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
