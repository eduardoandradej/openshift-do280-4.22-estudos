# Revisão — Capítulo 4

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Compare edge, passthrough e re-encrypt: onde o TLS termina e se o trecho router → Pod é cifrado.
2. Em que cenário o passthrough é necessário mesmo existindo o re-encrypt?
3. Por que, ao criar a primeira NetworkPolicy que seleciona um Pod, ele passa a recusar tráfego que antes aceitava?
4. Explique a diferença entre `namespaceSelector` e `podSelector` no mesmo item de `from` e em itens separados.
5. Por que, no cluster de nó único do curso, uma Route continua respondendo depois de um default-deny?
6. Compare NetworkPolicy, AdminNetworkPolicy e BaselineAdminNetworkPolicy quanto a escopo, autor e precedência.
7. O que o service-ca faz quando encontra um Service com `serving-cert-secret-name`? E um ConfigMap com `inject-cabundle`?
8. Por que rotacionar manualmente a CA do service-ca é uma operação de alto impacto?

## Parte 2 — Comandos (escreva o comando)

9. Criar uma Route edge `pagamentos-https` para o Service `pagamentos`, host `pagamentos.apps.exemplo.com`, com `pag.crt` e `pag.key`.
10. Criar um Secret TLS `pag-tls` a partir dos mesmos arquivos.
11. Criar uma Route passthrough `pagamentos-tls` para o Service `pagamentos-tls`, porta `8443`, host `pagamentos-tls.apps.exemplo.com`.
12. Gerar o YAML de uma Route edge sem criá-la.
13. Rotular o namespace `relatorios` com `zona=interna`.
14. Listar as NetworkPolicies do projeto `loja` e ver o resumo legível da `api-aceita-web`.
15. Pedir ao OpenShift um certificado de serviço `api-cert` para o Service `api`.
16. Criar o ConfigMap `ca-servicos` e fazer o service-ca injetar nele o bundle da CA.
17. Testar, de dentro do Deployment `cliente`, a conexão TLS com `api.loja.svc` na porta 443 e ver o código de verificação.

## Parte 3 — Diagnóstico

18. Após habilitar TLS na aplicação, os Pods novos ficam `0/1` e o rollout não termina. Causa provável?
19. Uma política foi criada para liberar `web → api`, mas o `curl` de `web` para `api` continua sem resposta. Cite três verificações.
20. Um cliente interno recebe `self signed certificate in certificate chain` ao chamar `https://api.loja.svc`. O que falta?
21. Ao aplicar uma AdminNetworkPolicy com `matchLabels: {restrito: true}`, o `oc apply` recusa com `cannot unmarshal bool`. Corrija.
22. Um colega anotou com `inject-cabundle` o ConfigMap de configuração da aplicação, que tinha outras chaves. O que aconteceu?
23. `oc create route edge web --service web --dry-run=server` criou a Route. Por quê, e como gerar só o manifesto?

---

<details>
<summary>Respostas comentadas</summary>

1. Edge: termina no router; router → Pod em claro. Passthrough: termina no Pod; o router não decifra (a conexão inteira é cifrada). Re-encrypt: termina no router e abre novo TLS até o Pod (cifrado).
2. Quando a aplicação precisa terminar o TLS ela mesma, por exemplo para autenticação mútua (mTLS) com o certificado do cliente, ou quando as chaves não podem ficar no router.
3. Porque um Pod selecionado por qualquer política de ingress passa a aceitar apenas o que alguma política permite (isolamento); políticas são aditivas e não há regra de negação.
4. No mesmo item: E (Pods com o label X **dentro** de namespaces com o label Y). Em itens separados: OU (todos os Pods dos namespaces Y, ou Pods X do namespace da própria política).
5. Os routers usam a rede do host e estão no mesmo nó dos Pods; NetworkPolicy não filtra esse tráfego no mesmo nó.
6. NP: namespace, criada pelo projeto, só permite, Tier 2. ANP: cluster, administrador, `Allow/Deny/Pass`, avaliada primeiro (Tier 1) e não sobreponível por NP. BANP: cluster, administrador, Tier 3, padrão que as NPs podem sobrepor.
7. No Service: cria no mesmo namespace um Secret TLS com certificado assinado pela CA do service-ca para os nomes DNS do Service. No ConfigMap: substitui o conteúdo pelo bundle da CA na chave `service-ca.crt`.
8. A CA anterior é invalidada na hora para todo o cluster; todos os Pods que usam certificados de serviço precisam ser reiniciados.
9. `oc create route edge pagamentos-https --service pagamentos --hostname pagamentos.apps.exemplo.com --cert pag.crt --key pag.key`
10. `oc create secret tls pag-tls --cert pag.crt --key pag.key`
11. `oc create route passthrough pagamentos-tls --service pagamentos-tls --port 8443 --hostname pagamentos-tls.apps.exemplo.com`
12. `oc create route edge web --service web --dry-run=client -o yaml`
13. `oc label namespace relatorios zona=interna`
14. `oc get networkpolicies -n loja` e `oc describe networkpolicy api-aceita-web -n loja`
15. `oc annotate service api service.beta.openshift.io/serving-cert-secret-name=api-cert`
16. `oc create configmap ca-servicos` e `oc annotate configmap ca-servicos service.beta.openshift.io/inject-cabundle=true`
17. `oc exec deploy/cliente -- openssl s_client -connect api.loja.svc:443 </dev/null | grep 'return code'`
18. As probes continuam em HTTP; com TLS obrigatório, falham. Use `scheme: HTTPS` (na mesma alteração que habilita o TLS).
19. A política está no namespace do **destino**? Os labels dos Pods (`--show-labels`) e a porta batem com a política? `oc describe networkpolicy` mostra a regra como esperado (E × OU)? Há alguma ANP negando?
20. O cliente não confia na CA do service-ca: monte um ConfigMap dedicado com `inject-cabundle` e aponte a aplicação para o bundle.
21. Valores de labels são texto: `restrito: "true"`.
22. O service-ca substituiu todo o conteúdo pelo bundle da CA; as configurações da aplicação foram perdidas. Use um ConfigMap dedicado e restaure o original pelo manifesto.
23. Os subcomandos `oc create route edge|passthrough|reencrypt` não respeitam `--dry-run=server` (segundo a apostila). Use `--dry-run=client -o yaml` e apague a Route criada sem querer.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 21–23 | Siga para o Capítulo 5 |
| 16–20 | Refaça a Parte C das lições em que errou |
| < 16 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
