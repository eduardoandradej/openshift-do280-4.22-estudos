# Prática oficial — Capítulo 3

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`.
> Usuários, senhas, nomes de grupos, Secrets e projetos estão no enunciado de cada
> atividade. Nenhum comando `lab` é executado por este repositório.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | PDF |
|-------|------|---------------------|-------------|-----|
| 3.2 | Guided Exercise | `auth-providers` | Não | p. 119–128 |
| 3.4 | Guided Exercise | `auth-rbac` | Não | p. 134–137 |
| 3.5 | Lab | `auth-review` | **Sim** | p. 138–144 |

Identificadores e avaliação verificados no PDF do Capítulo 3. O capítulo não tem quiz.

> ⚠️ O cluster do curso **já tem um provedor LDAP** configurado (é por ele que o
> usuário administrador do laboratório entra). Ao acrescentar o HTPasswd, **preserve o
> item LDAP** em `spec.identityProviders`. E **nunca apague o `kubeadmin`** no curso.

> ⚠️ Mudanças no Secret ou no OAuth reimplantam os Pods `oauth-openshift`; isso pode
> levar vários minutos. Login falhando logo após a mudança é esperado: aguarde.

---

## 3.2 Guided Exercise: Configure Identity Providers

| Campo | Valor |
|-------|-------|
| Seção | 3.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `auth-providers` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 119–128 |

**Competência praticada:** criar um arquivo htpasswd, publicá-lo como Secret,
acrescentar um IdP HTPasswd ao OAuth, dar `cluster-admin` a um usuário, comprovar a
diferença de acesso entre usuários, e cumprir o ciclo completo de manutenção (incluir,
trocar senha, remover usuário com User e Identity) e de limpeza do IdP.

**Domine antes:** [3.1](3.1-provedor-de-identidade-htpasswd.md) (Partes A e C).

```bash
lab start auth-providers
lab finish auth-providers
```

**Pontos de atenção:**
- `htpasswd -c` só na primeira vez; depois, sempre extraia o Secret antes de editar.
- Confirme a chave `htpasswd` e o namespace `openshift-config` do Secret.
- O aviso de usuário inexistente ao conceder `cluster-admin` é normal.
- Repare no nome do ClusterRoleBinding criado (sufixo numérico).
- Ao final, a limpeza remove o IdP do OAuth: remova **só** o item que você adicionou.
- A limpeza também apaga **todos** os Users e Identities, inclusive o do administrador
  que entra pelo LDAP. Isso é esperado: eles são recriados no próximo login. O acesso
  administrativo continua porque as permissões estão nos bindings (pelo nome), não no
  objeto User.

**Verificação conceitual:** `oc get users` e `oc get identities` mostram os usuários do
novo IdP com o prefixo do nome do IdP; um usuário lista nós e o outro recebe `Forbidden`;
o usuário removido recebe `401` e não aparece mais em users/identities.

**Diagnóstico se algo falhar:** `oc get pods -n openshift-authentication` →
`oc get co authentication` → `oc get oauth cluster -o yaml` →
`oc extract secret/<s> -n openshift-config --to -`.

---

## 3.4 Guided Exercise: Define and Apply Permissions with RBAC

| Campo | Valor |
|-------|-------|
| Seção | 3.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `auth-rbac` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 134–137 |

**Competência praticada:** como administrador, criar um projeto e delegar sua
administração; criar grupos e membros; como administrador do projeto, conceder escrita
a um grupo e leitura a outro; e comprovar cada nível de acesso, inclusive as tentativas
que **devem** falhar.

**Domine antes:** [3.3](3.3-permissoes-com-rbac.md) (papéis padrão, `oc policy`,
`oc adm groups`, leitura de `Forbidden`).

```bash
lab start auth-rbac
lab finish auth-rbac
```

**Pontos de atenção:**
- Note quem executa cada passo (administrador do cluster × administrador do projeto × membros).
- O ambiente já tem grupos vindos do LDAP; não os confunda com os que você cria.
- Leia cada mensagem `Forbidden`: ela diz o verbo, o recurso e o namespace que faltaram.
- Filtre `oc get rolebindings -o wide` para enxergar só os vínculos não sistêmicos.

**Verificação conceitual:** o grupo de escrita cria recursos mas não concede acessos;
o grupo de leitura lista mas não escala/altera; o administrador do projeto gerencia ambos.

**Diagnóstico se algo falhar:** `oc get groups` → `oc get rolebindings -o wide -n <p>`
→ 📘 `oc auth can-i <verbo> <recurso> -n <p>` executado **como o próprio usuário** testado.
A forma com `--as <u> --as-group <g>` só funciona logado como `admin`: usuários comuns
recebem `cannot impersonate` (não é falha do exercício).

---

## 3.5 Lab: Configure Authentication and Authorization

| Campo | Valor |
|-------|-------|
| Seção | 3.5 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `auth-review` |
| Avaliação | **`lab grade auth-review`** |
| Referência | PDF p. 138–144 (enunciado p. 138–139; solução a partir da p. 140) |

**Competência praticada (resumo autoral):** a partir de um arquivo htpasswd existente,
remover, atualizar e incluir usuários; configurar o IdP HTPasswd com um Secret de nome
definido; tornar um usuário administrador do cluster; criar grupos, fazer um membro de
um grupo criar o projeto e conceder papéis de escrita e leitura a outros grupos no
projeto.

**Domine antes:** as duas lições, os dois guided exercises e a [revisão](REVISAO.md).

```bash
lab start auth-review
lab grade auth-review
lab finish auth-review
```

**Pontos de atenção:**
- Nomes de Secret, grupos, projeto e papéis são conferidos pelo `lab grade`: use exatamente os do enunciado.
- O arquivo htpasswd é **fornecido**: atualize-o, não o recrie com `-c`.
- Senhas com caracteres especiais exigem aspas simples no shell.
- Preserve o IdP LDAP ao editar o OAuth; acompanhe a reimplantação dos Pods antes de testar logins.
- Lembre que quem cria o projeto recebe `admin` nele automaticamente.

**Verificação conceitual:** cada usuário entra pelo novo IdP; só o administrador lista
nós; os grupos têm os papéis pedidos no projeto (`oc get rolebindings -o wide`).

**Diagnóstico se algo falhar:** saída do `lab grade` → `oc get oauth cluster -o yaml`
→ `oc extract secret/<s> -n openshift-config --to -` → `oc get groups` →
`oc get rolebindings -o wide -n <projeto>`.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 3.3 Permissões com RBAC](3.3-permissoes-com-rbac.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
