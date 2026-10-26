# Revisão — Capítulo 9

Questões **autorais**. Elas não reproduzem os quizzes oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Explique, com suas palavras, como funciona uma atualização over-the-air do OpenShift.
2. Uma empresa tem clusters de desenvolvimento, homologação e produção. Que canal usar em cada um, e por quê?
3. O que muda quando um cluster 4.22 passa a seguir o canal `eus-4.22`?
4. Que papel têm o CVO, o MCO e o OLM durante a atualização de um cluster?
5. Cite quatro verificações a fazer antes de atualizar um cluster de produção.
6. Qual a diferença entre uma API obsoleta e uma API removida, do ponto de vista de quem aplica manifestos?
7. Por que trocar o canal de um operator não basta para atualizá-lo?
8. O que é o OLM v1 e o que muda na forma de atualizar operators?

## Parte 2 — Comandos (escreva o comando)

9. Mostrar a versão atual, o canal e as atualizações recomendadas.
10. Trocar o canal do cluster para `stable-4.22` (duas formas).
11. Pausar o MachineHealthCheck `mhc-workers` e, depois, retomar.
12. Atualizar o cluster para a versão `4.22.9`, se listada, e acompanhar o progresso.
13. Listar só as APIs obsoletas que receberam requisições.
14. Ver a versão instalada e a mais nova disponível do operator assinado por `auditoria` no namespace `auditoria-op`.
15. Aprovar o install plan `install-k2j7p` desse namespace.
16. Ver a versão instalada de um operator gerenciado pela `ClusterExtension` `auditoria`.

## Parte 3 — Diagnóstico

17. `oc adm upgrade` mostra `Upgradeable=False` com `IncompatibleOperatorsInstalled`. O que fazer?
18. Um pipeline passou a falhar com `no matches for kind "FlowSchema" in version "flowcontrol.apiserver.k8s.io/v1beta3"` depois da atualização. Causa e correção.
19. O alerta `APIRemovedInNextReleaseInUse` está disparado. Como descobrir o culpado?
20. A atualização está em `PROGRESSING True` há horas, com um cluster operator degradado. Pode-se voltar à versão anterior? O que fazer?
21. Durante uma atualização, nós de computação foram reiniciados fora de hora. O que foi esquecido?
22. Um operator de aprovação manual mostra "Upgrade available" há semanas. Por que não atualizou?

---

<details>
<summary>Respostas comentadas</summary>

1. A Red Hat publica imagens de release e um serviço calcula os caminhos de atualização por canal; o administrador escolhe a versão e o CVO aplica a release, atualizando os cluster operators e, via MCO, o SO dos nós em rolling update.
2. Desenvolvimento: `fast` (ou `candidate` só para testes, sem suporte); homologação: `fast` ou `stable`; produção: `stable` (ou `eus` em versões pares, para ciclo longo).
3. Ao entrar na fase EUS, o cluster passa a receber só correções críticas e de segurança, com suporte estendido; antes disso, `stable` e `eus` oferecem o mesmo.
4. CVO orquestra a release e atualiza os cluster operators; MCO atualiza configuração e SO dos nós com reinício em rolling; OLM atualiza os operators adicionais conforme as Subscriptions (separadamente).
5. Operators compatíveis com o destino; nenhuma API removida em uso; cluster operators saudáveis; MHC pausados (exceto nó único); boot image acima do mínimo; caso de suporte aberto em clusters críticos.
6. A obsoleta funciona com aviso; a removida gera erro porque o cluster não serve mais aquela versão.
7. A troca só muda as versões visíveis; a atualização ocorre se houver versão mais nova no canal e a aprovação for automática (ou após aprovar o install plan).
8. Um modelo declarativo baseado em `ClusterExtension`, sem Subscription/InstallPlan; atualiza-se editando `version` (fixa ou faixa) ou `channels` e aplicando; na 4.22, só pela CLI.
9. `oc adm upgrade` (e `oc get clusterversion`)
10. `oc patch clusterversion version --type merge -p '{"spec":{"channel":"stable-4.22"}}'` ou `oc adm upgrade channel stable-4.22`
11. `oc annotate machinehealthcheck mhc-workers -n openshift-machine-api cluster.x-k8s.io/paused=""` e `oc annotate machinehealthcheck mhc-workers -n openshift-machine-api cluster.x-k8s.io/paused-`
12. `oc adm upgrade --to=4.22.9` e `watch oc get clusterversion,clusteroperators`
13. `oc get apirequestcounts -o jsonpath='{range .items[?(@.status.removedInRelease!="")]}{.status.removedInRelease}{"\t"}{.metadata.name}{"\n"}{end}'`
14. `oc get subscription auditoria -n auditoria-op -o jsonpath='{.status.installedCSV}{" -> "}{.status.currentCSV}{"\n"}'`
15. `oc patch installplan install-k2j7p -n auditoria-op --type merge -p '{"spec":{"approved":true}}'`
16. `oc get clusterextension auditoria -o jsonpath='{.status.install.bundle.version}{"\n"}'`
17. Atualizar o operator para uma versão compatível com o destino; se não existir, desinstalar, atualizar o cluster e reinstalar.
18. A versão `v1beta3` foi removida; atualize o manifesto para `flowcontrol.apiserver.k8s.io/v1`.
19. Pelo `APIRequestCount` do recurso/versão: o `status` mostra usuário, `userAgent` e verbos por hora.
20. Não: rollback não é suportado. Investigar o operator degradado (`oc describe co`) e acionar o suporte; o CVO segue tentando reconciliar.
21. Pausar os MachineHealthChecks antes da atualização.
22. Com aprovação manual, o install plan fica aguardando `approved: true`.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 10 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Releia as lições e refaça os quizzes; registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
