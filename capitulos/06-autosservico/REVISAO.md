# Revisão — Capítulo 6

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Por que requests e limits por container não impedem que um projeto ocupe o cluster?
2. Compare quota de computação e quota de contagem de objetos, com um exemplo de cada.
3. Qual a diferença entre ResourceQuota e ClusterResourceQuota? E onde entra o AppliedClusterResourceQuota?
4. Qual o papel de `default`, `defaultRequest`, `min` e `max` em um LimitRange?
5. Em que momento um limit range age, e sobre qual objeto?
6. Por que limit ranges e quotas costumam andar juntos?
7. O que `Project` e `ProjectRequest` resolvem que o namespace puro não resolve?
8. Para que serve a anotação `rbac.authorization.kubernetes.io/autoupdate`?

## Parte 2 — Comandos (escreva o comando)

9. Criar no projeto `loja` a quota `memoria` com 3Gi de requests e 6Gi de limits de memória.
10. Criar no projeto `loja` a quota `objetos` limitando a 4 os Deployments e a 2 os PVCs.
11. Criar a cluster quota `time-azul` com no máximo 30 Pods para os projetos com label `time=azul`.
12. Como administrador do projeto `azul-dev`, ver o uso da cluster quota do item anterior.
13. Gerar um template de projeto inicial no arquivo `projeto.yaml`.
14. Publicar o template do item anterior para uso na criação de projetos.
15. Apontar a configuração global de projetos para o template `project-request` sem editor interativo.
16. Tornar permanente uma alteração no binding `self-provisioners`.

## Parte 3 — Diagnóstico

17. Um Deployment novo fica `READY 0/1`, sem Pods. Os eventos dizem `failed quota: ... must specify limits.memory`. Duas correções possíveis.
18. Uma quota `count/deployment: "2"` não impede o terceiro Deployment. Por quê?
19. Um limit range foi criado ontem, mas os Pods de um Deployment antigo continuam sem limits. Explique e corrija.
20. Um limit range tem apenas `min` e `max` de memória. Containers sem `resources` estão consumindo a quota rapidamente. Por quê?
21. Depois de configurar o template, você cria um projeto e ele não tem a quota esperada. O que conferir, em ordem?
22. Você restringiu `self-provisioners` ao grupo `plataforma`; dias depois, todos voltaram a criar projetos. Causa e correção.

---

<details>
<summary>Respostas comentadas</summary>

1. Eles limitam cada container, não a quantidade de workloads; muitas réplicas com requests altos ocupam a capacidade reservável do nó e deixam outros projetos `Pending`.
2. Computação soma CPU/memória (ex.: `requests.cpu: "4"`); contagem limita o número de objetos (ex.: `count/deployments.apps: "10"`).
3. ResourceQuota vale para um namespace; ClusterResourceQuota (OpenShift) soma vários namespaces escolhidos por selector. O AppliedClusterResourceQuota é a visão, por namespace, que o administrador do projeto consegue ler.
4. `default`: limit injetado; `defaultRequest`: request injetado; `min`/`max`: recusam valores fora da faixa (para request e limit).
5. Na criação do Pod (admission controller do API server). O Deployment não é alterado e Pods existentes não mudam.
6. Com quota de computação, containers sem request/limit são recusados; os padrões do limit range os preenchem.
7. Usuários criam projetos sem editar metadados de namespaces nem listar todos eles; o administrador controla o que cada projeto recebe por meio do template.
8. Com `true`, o API server restaura papéis e bindings protegidos ao iniciar; com `false`, alterações manuais persistem.
9. `oc create quota memoria -n loja --hard requests.memory=3Gi,limits.memory=6Gi`
10. `oc create quota objetos -n loja --hard count/deployments.apps=4,count/persistentvolumeclaims=2`
11. `oc create clusterresourcequota time-azul --project-label-selector time=azul --hard pods=30`
12. `oc describe AppliedClusterResourceQuota -n azul-dev`
13. `oc adm create-bootstrap-project-template -o yaml > projeto.yaml`
14. `oc create -f projeto.yaml -n openshift-config` (seguido da configuração em `projects.config.openshift.io/cluster`)
15. `oc patch projects.config.openshift.io cluster --type merge -p '{"spec":{"projectRequestTemplate":{"name":"project-request"}}}'`
16. `oc annotate clusterrolebinding/self-provisioners --overwrite rbac.authorization.kubernetes.io/autoupdate=false`, e então editar os `subjects`.
17. Declarar o recurso no workload (`oc set resources deployment <d> --limits memory=...`) ou criar um LimitRange com `default` de memória (e recriar os Pods).
18. A chave está errada (singular, sem grupo) e quotas não são validadas semanticamente; use `count/deployments.apps`.
19. O limit range só age na criação de Pods. Faça um novo rollout (`oc rollout restart deployment/<d>`).
20. Sem padrões declarados, `default` herda de `max` e `defaultRequest` herda de `default`: cada container reserva o máximo. Declare `defaultRequest` e `default`.
21. Se o template está em `openshift-config` com o nome configurado; se `projects.config.openshift.io/cluster` aponta para ele; se o rollout do `apiserver` em `openshift-apiserver` terminou antes da criação; se a chave da quota está correta.
22. A anotação `autoupdate` continuou `true` e o API server reiniciou, restaurando o binding. Anote `autoupdate=false` e refaça a troca de sujeito.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 7 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
