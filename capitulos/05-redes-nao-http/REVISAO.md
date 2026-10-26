# Revisão — Capítulo 5

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Por que Routes e Ingress expõem HTTP e TLS com SNI, mas não SSH ou PostgreSQL?
2. Compare `ClusterIP`, `NodePort` e `LoadBalancer`.
3. Quem implementa um Service `LoadBalancer` na nuvem e em bare metal?
4. Qual o papel de `IPAddressPool` e `L2Advertisement` no MetalLB? O que muda no modo BGP?
5. Compare UDN, CUDN e NAD quanto a escopo, autor e uso.
6. Explique a diferença entre UDN primária e secundária.
7. Qual a diferença entre as topologias Layer2 e Layer3 de uma UDN?
8. Por que um namespace com UDN primária precisa ser planejado desde a criação?

## Parte 2 — Comandos (escreva o comando)

9. Expor o Deployment `sftp` com um Service `LoadBalancer` chamado `sftp-lb` na porta 22.
10. Mostrar só o IP externo do Service `sftp-lb`.
11. Testar se a porta 22 do IP externo `192.168.77.21` aceita conexões.
12. Listar os pools de IP do MetalLB.
13. Ver as condições da UDN `udn-app` do namespace `app-isolado`.
14. Listar todas as NADs gerenciadas por UDN/CUDN no cluster.
15. Mostrar em quais redes está o Pod `web-abc` e qual interface é a padrão.
16. Conferir se a interface `ens5` existe no nó `worker-1`.

## Parte 3 — Diagnóstico

17. Um novo Service `LoadBalancer` está em `<pending>` há 10 minutos, e outros funcionam. Causa provável e ação.
18. Os Pods de um namespace não sobem; o evento diz `invalid primary network state for namespace`. O que falta?
19. A UDN foi criada, mas as condições indicam falha. O que verificar primeiro?
20. Depois de anotar o template com `k8s.v1.cni.cncf.io/networks: "rede-a, rede-b"`, o Pod não sobe. Por quê?
21. Uma CUDN falha com `foreign NetworkAttachmentDefinition with the desired name already exist`. Corrija.
22. Após atualizar um Deployment que usa NAD `host-device`, o Pod novo fica preso. Explique e corrija.

---

<details>
<summary>Respostas comentadas</summary>

1. Elas roteiam pelo nome do host (HTTP) ou pelo SNI (TLS), compartilhando IP e porta; protocolos como SSH e PostgreSQL não carregam esse nome, então cada serviço precisa de IP/porta próprios.
2. `ClusterIP`: só interno. `NodePort`: porta alta em todos os nós (`IP-do-nó:porta`). `LoadBalancer`: IP externo dedicado na porta do serviço, construído sobre os outros dois.
3. Na nuvem, o cloud controller manager cria um balanceador do provedor (pago). Em bare metal, um operador como o MetalLB atribui IPs de um pool e os anuncia.
4. O pool define quais IPs podem ser atribuídos; o L2Advertisement os anuncia por ARP (um nó responde pelo IP). No BGP, os nós anunciam rotas a roteadores peers (exige número de AS).
5. UDN: namespace, administrador do projeto, rede isolada gerenciada pelo OVN-Kubernetes. CUDN: cluster, administrador do cluster, uma rede para vários namespaces. NAD: namespace, configuração CNI para redes externas ou plug-ins específicos.
6. Primária: interface padrão de todos os Pods do namespace. Secundária: interface extra, pedida por anotação, só para o tráfego daquela rede.
7. Layer2: uma sub-rede para todos os nós. Layer3: a sub-rede é dividida e cada nó recebe um pedaço.
8. O label `k8s.ovn.org/primary-user-defined-network` só pode ser definido na criação, e nenhum Pod sobe até a UDN existir.
9. `oc expose deployment/sftp --name sftp-lb --type LoadBalancer --port 22 --target-port 22`
10. `oc get svc sftp-lb -o jsonpath='{.status.loadBalancer.ingress[0].ip}{"\n"}'`
11. `nc -vz 192.168.77.21 22`
12. `oc get ipaddresspool -n metallb-system`
13. `oc get userdefinednetwork udn-app -n app-isolado -o yaml` (seção `status.conditions`) ou `oc describe userdefinednetwork udn-app -n app-isolado`
14. `oc get net-attach-def -A -l k8s.ovn.org/user-defined-network`
15. `oc get pod web-abc -o jsonpath='{.metadata.annotations.k8s\.v1\.cni\.cncf\.io/network-status}'` (a padrão tem `"default": true`)
16. `oc debug node/worker-1 -- chroot /host ip addr show ens5`
17. O pool do MetalLB provavelmente se esgotou. Liste os Services `LoadBalancer` (`oc get svc -A | grep LoadBalancer`), libere um que não é mais necessário ou peça ao administrador para ampliar o pool.
18. A UDN do namespace: com o label de UDN primária, os Pods só sobem quando ela existe.
19. Se o namespace tem o label de UDN primária (`oc get ns <ns> --show-labels`); sem ele a rede não é provisionada, e o label exige recriar o namespace.
20. A lista não pode ter espaços: use `rede-a,rede-b`.
21. Use um nome de CUDN que não colida com NADs existentes nos namespaces selecionados.
22. A interface física só pode estar em um Pod; com `RollingUpdate`, o novo espera o antigo liberá-la. Use `strategy: Recreate` (e uma réplica).

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 6 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
