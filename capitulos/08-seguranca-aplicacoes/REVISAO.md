# Revisão — Capítulo 8

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Compare RBAC e SCC: o que cada um controla e a quem se aplica?
2. Por que muitas imagens públicas falham com a SCC `restricted-v2`? Cite três motivos.
3. Por que a SCC é concedida à service account, e não ao Deployment?
4. Qual a diferença de escopo entre `oc adm policy add-scc-to-user -z <sa>` e uma Role com verbo `use` na SCC?
5. Para que serve a anotação `openshift.io/required-scc`?
6. Por que não se deve dar permissões à service account `default`?
7. Onde fica o RoleBinding quando a aplicação de um projeto precisa ler recursos de outro?
8. Qual a relação entre CronJob, Job e Pod, e onde a service account é declarada no CronJob?

## Parte 2 — Comandos (escreva o comando)

9. Descobrir qual SCC permitiria o Deployment `legado` do projeto atual.
10. Criar a SA `legado-sa`, conceder a ela a SCC `nonroot-v2` (atalho) e associá-la ao Deployment `legado`.
11. Permitir que a SA `legado-sa` use a SCC `anyuid` **somente** no projeto `fin`.
12. Conceder à SA `coletor` do projeto `observa` o papel `view` no projeto `vendas`, com o binding `coletor-view`.
13. Verificar se essa SA pode listar Pods em `vendas`.
14. Gerar um token de 30 minutos para a SA `pipeline` do projeto atual.
15. Gerar o YAML de um CronJob `relatorio` que roda `date` toda segunda às 07:00 com a imagem `ubi9`.
16. Executar agora o CronJob `relatorio`, sem esperar o horário.

## Parte 3 — Diagnóstico

17. Um Pod está em `CrashLoopBackOff` e o log mostra `Permission denied` ao criar `/var/lib/app`. A anotação diz `openshift.io/scc: restricted-v2`. Causa e correção.
18. Você concedeu `anyuid` à SA `web-sa`, mas o Pod continua com `restricted-v2`. O que faltou?
19. Um controller registra `User "system:serviceaccount:ferramentas:default" cannot list resource "configmaps"`. Quais as correções?
20. O mesmo controller, já com SA própria, registra `... cannot list resource "nodes" ... at the cluster scope`. O que conceder?
21. Um CronJob de manutenção sempre termina `Failed` com `Forbidden`. Onde estão as duas permissões que costumam faltar?
22. `oc apply` de um ConfigMap falha com `unknown field "metadata.app"`. O que está errado?

---

<details>
<summary>Respostas comentadas</summary>

1. RBAC controla ações na API (usuários e SAs); SCC controla privilégios do Pod no host (UID, capabilities, SELinux, volumes e rede do host) e é avaliada na admissão do Pod com base na SA.
2. Exigem UID fixo ou root; escrevem em diretórios do sistema; escutam em portas abaixo de 1024.
3. Porque o Pod é admitido com a identidade da SA; todo Pod que a usar recebe a SCC.
4. O atalho cria um ClusterRoleBinding (sem `-n` explícito), válido em todo o cluster; Role + RoleBinding limitam ao projeto.
5. Fixar a SCC que o Pod deve receber; a SCC precisa existir e a SA precisa poder usá-la.
6. Todo Pod do projeto que não declara SA a usa e herdaria as permissões.
7. No projeto dos recursos, com sujeito `system:serviceaccount:<proj-da-app>:<sa>`.
8. CronJob cria Jobs; Job cria Pods. A SA fica em `spec.jobTemplate.spec.template.spec.serviceAccountName`.
9. `oc get deployment legado -o yaml | oc adm policy scc-subject-review -f -`
10. `oc create sa legado-sa`, `oc adm policy add-scc-to-user nonroot-v2 -z legado-sa` e `oc set serviceaccount deployment/legado legado-sa`
11. `oc create role usa-anyuid --verb use --resource scc --resource-name anyuid -n fin` e `oc create rolebinding usa-anyuid --role usa-anyuid --serviceaccount fin:legado-sa -n fin`
12. `oc adm policy add-role-to-user view system:serviceaccount:observa:coletor --rolebinding-name coletor-view -n vendas`
13. `oc auth can-i list pods -n vendas --as system:serviceaccount:observa:coletor`
14. `oc create token pipeline --duration 30m`
15. `oc create cronjob relatorio --image ubi9 --schedule '0 7 * * 1' --dry-run=client -o yaml -- date`
16. `oc create job relatorio-manual --from cronjob/relatorio`
17. A imagem espera root (ou UID fixo) e o UID aleatório não pode escrever ali. Melhor: ajustar a imagem; senão, SA com a SCC mínima (`scc-subject-review`), concedida e associada ao Deployment.
18. Associar a SA ao Deployment (`oc set serviceaccount`), que recria os Pods.
19. Criar uma SA para o controller, associá-la ao Deployment e conceder um papel com leitura de ConfigMaps no namespace dos ConfigMaps.
20. Uma ClusterRole que permita listar `nodes`, ligada por ClusterRoleBinding (recurso de cluster).
21. No papel de API da SA (ClusterRoleBinding) e na SCC dos Pods que ela cria; e a SA precisa estar em `serviceAccountName` do template.
22. Um campo foi posto direto em `metadata`; labels ficam em `metadata.labels`.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 9 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
