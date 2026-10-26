# Revisão — Capítulo 3

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Quais são os dois métodos de autenticação da API do OpenShift, e o que acontece com uma requisição sem nenhum deles?
2. Qual o papel do servidor OAuth e dos provedores de identidade no login?
3. Qual a diferença entre User e Identity, e quando eles são criados?
4. Por que o HTPasswd não é recomendado para produção?
5. Compare o kubeconfig do instalador e o `kubeadmin`. Em que condição é seguro remover o `kubeadmin`?
6. Explique regra, papel e vínculo, e a diferença entre RoleBinding e ClusterRoleBinding.
7. Por que conceder uma ClusterRole com um RoleBinding é uma prática comum?
8. Compare os papéis `admin`, `edit` e `view` em um projeto.

## Parte 2 — Comandos (escreva o comando)

9. Criar o arquivo `~/idp/usuarios` com o usuário `ana` (bcrypt, senha na linha de comando).
10. Acrescentar o usuário `bruno` ao mesmo arquivo.
11. Criar o Secret `usuarios-htpasswd` com esse arquivo, no lugar onde o OAuth o procura.
12. Trazer o conteúdo atual desse Secret para o diretório `~/idp`, sobrescrevendo.
13. Devolver o arquivo atualizado ao Secret.
14. Acompanhar a reimplantação do servidor OAuth.
15. Dar ao usuário `ana` acesso total ao cluster.
16. Criar o grupo `analistas` e incluir `bruno`.
17. Dar ao grupo `analistas` leitura no projeto `financeiro`.
18. Descobrir quem pode criar ClusterRoleBindings.

## Parte 3 — Diagnóstico

19. Após configurar o HTPasswd, ninguém consegue mais entrar pelo LDAP. Causa provável e correção.
20. Um usuário recém-incluído recebe `401` por alguns minutos e depois entra normalmente. É um problema?
21. `bruno` recebe: `User "bruno" cannot create resource "deployments" in API group "apps" in the namespace "financeiro"`. O que isso indica e o que você verifica?
22. `carla` foi removida do arquivo htpasswd e do Secret, mas ainda aparece em `oc get users`. O que falta? Existe algum risco residual?
23. Um líder de projeto com `edit` não consegue dar acesso a um colega. Por quê?

---

<details>
<summary>Respostas comentadas</summary>

1. Tokens de acesso OAuth e certificados de cliente X.509. Sem nenhum, a requisição é atribuída a `system:anonymous`, no grupo `system:unauthenticated`.
2. O servidor OAuth recebe o login, pede ao IdP configurado no recurso OAuth que valide a credencial, reconcilia Identity/User e emite o token de acesso.
3. User representa o ator no OpenShift; Identity registra o vínculo dele com um IdP (`idp:usuario`). Ambos são criados automaticamente no primeiro login bem-sucedido.
4. É um arquivo local mantido manualmente pelo administrador, sem integração com o diretório da organização; usuários não trocam a própria senha.
5. O kubeconfig usa um certificado X.509 e não depende do OAuth; o `kubeadmin` é um usuário virtual que obtém token OAuth. Só se remove o `kubeadmin` depois de outro usuário com `cluster-admin` estar funcionando (e nunca no ambiente do curso).
6. Regra = verbos sobre recursos de um grupo de API; papel = conjunto de regras; vínculo = associação de um papel a sujeitos. RoleBinding vale em um projeto; ClusterRoleBinding, no cluster todo.
7. Reaproveita uma definição única (ex.: `edit`) e limita o alcance ao projeto do RoleBinding.
8. `admin` gerencia todo o projeto, inclusive acessos, quotas e limit ranges; `edit` cria e altera recursos de aplicação, sem gerenciar acessos; `view` só lê.
9. `htpasswd -c -B -b ~/idp/usuarios ana 'troque-me'`
10. `htpasswd -B -b ~/idp/usuarios bruno 'troque-me'`
11. `oc create secret generic usuarios-htpasswd --from-file htpasswd=$HOME/idp/usuarios -n openshift-config`
12. `oc extract secret/usuarios-htpasswd -n openshift-config --to ~/idp --confirm` (o arquivo extraído se chamará `htpasswd`)
13. `oc set data secret/usuarios-htpasswd -n openshift-config --from-file htpasswd=$HOME/idp/htpasswd`
14. `watch oc get pods -n openshift-authentication`
15. `oc adm policy add-cluster-role-to-user cluster-admin ana`
16. `oc adm groups new analistas` e `oc adm groups add-users analistas bruno`
17. `oc policy add-role-to-group view analistas -n financeiro`
18. `oc adm policy who-can create clusterrolebindings`
19. A lista `spec.identityProviders` foi substituída sem o item LDAP (arquivo editado sem ele ou merge patch). Restaure o item LDAP e, para acrescentar IdPs, edite o objeto completo ou use JSON Patch `add` em `/spec/identityProviders/-`.
20. Não: os Pods `oauth-openshift` estavam sendo reimplantados com a nova configuração.
21. Ele está autenticado, mas não tem permissão de criar Deployments no projeto (provavelmente só `view` ou nenhum papel). Verifique `oc get rolebindings -o wide -n financeiro`, a participação dele nos grupos (`oc get groups`) e 📘 `oc auth can-i create deployments -n financeiro --as bruno`.
22. Faltam `oc delete user carla` e `oc delete identity <idp>:carla`. Risco residual: RoleBindings/ClusterRoleBindings que citam `carla` pelo nome continuam existindo e valeriam para um futuro usuário com o mesmo nome; revise-os.
23. Conceder acesso cria RoleBindings, o que exige o papel `admin` no projeto; `edit` não permite.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 21–23 | Siga para o Capítulo 4 |
| 16–20 | Refaça a Parte C das lições em que errou |
| < 16 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
