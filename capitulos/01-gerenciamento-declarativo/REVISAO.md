# Revisão — Capítulo 1

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Cite três desvantagens de manter uma aplicação apenas com comandos imperativos.
2. Em que situação o fluxo imperativo é **mais** útil que o declarativo, e por quê?
3. Quais são as três fontes que o `oc apply` compara, e para que serve cada uma?
4. Por que `--dry-run=server` encontra erros que `--dry-run=client` não encontra?
5. Por que `oc diff` é preferível a comparar `oc get -o yaml` com o arquivo no Git?
6. Explique a diferença entre base e overlay no Kustomize.
7. Por que um `configMapGenerator` resolve o problema de Pods com configuração antiga?
8. O que muda entre `labels` e `commonLabels`, e por que isso pode quebrar um `apply`?

## Parte 2 — Comandos (escreva o comando)

9. Gerar, sem criar nada, o manifesto de um Deployment `relatorios` com a imagem `registry.exemplo.com/relatorios:1.0`, gravando em `relatorios.yaml` com a anotação usada pelo `apply`.
10. Criar todos os recursos descritos em `~/app/` e em seus subdiretórios.
11. Validar no servidor todos os manifestos de `~/app/` sem persistir nada.
12. Mostrar o que mudaria no cluster se `~/app/` fosse aplicado.
13. Recriar os Pods do Deployment `relatorios` para que leiam um Secret atualizado.
14. Mudar para `200m` o request de CPU do container `app` do Deployment `relatorios` com um único comando.
15. Ver o YAML que o overlay `overlays/homologacao` geraria.
16. Aplicar o overlay `overlays/homologacao` e, depois, removê-lo.

## Parte 3 — Diagnóstico

17. `oc create -f app/` funcionou ontem; hoje retorna `AlreadyExists` para todos os objetos. O que fazer?
18. Após `oc apply -k overlays/producao`, a saída mostra `secret/db-<novo-hash> created` e os Deployments `configured`, mas um Secret antigo continua listado. É um problema?
19. Um overlay com `commonLabels: {tier: web}` aplicado sobre um Deployment já existente falha com `field is immutable`. Explique e corrija.
20. Depois de acrescentar `nameSuffix: -v2` a um overlay, a Route da aplicação passou a responder com erro. Como confirmar a causa e corrigir?
21. Você removeu a variável `MODO_DEBUG` do manifesto e aplicou, mas ela continua no Deployment. Qual a causa provável e como confirmar?
22. Um patch strategic merge deveria trocar o caminho de montagem do volume `dados` de `/data` para `/srv/dados`, mas agora o container tem os dois caminhos. Por quê, e como corrigir?

---

<details>
<summary>Respostas comentadas</summary>

1. Reprodutibilidade prejudicada (depende de repetir a sequência), ausência de controle de versão (nada a revisar ou reverter) e falta de suporte a GitOps/CI/CD (não há arquivo para o pipeline aplicar).
2. Ao desenvolver e experimentar: adiciona-se um componente por vez e o erro aponta para aquele componente, como um *debugger* passo a passo.
3. Manifesto atual (o desejado), objeto vivo (o real, com campos de controllers) e a anotação `last-applied-configuration` (o último manifesto aplicado, para saber o que foi **removido** de propósito).
4. Porque o servidor executa validação de schema da versão real da API, admission controllers, quotas, políticas e permissões; o modo client só monta o objeto.
5. O cluster acrescenta anotações, `status`, padrões e `managedFields`; um diff textual vira ruído. O `oc diff` compara o vivo com o resultado do apply, mostra só o que importa e mascara Secrets.
6. A base contém os recursos comuns; o overlay referencia a base e declara apenas as diferenças de um ambiente (namespace, réplicas, patches...). A base não é alterada.
7. O nome do objeto gerado recebe um hash do conteúdo; mudou o conteúdo, muda o nome, muda o template do Deployment e há rollout automático.
8. `labels` com `pairs` só adiciona em `metadata.labels` (por padrão); `commonLabels` (obsoleto) também altera selectors e templates. Como o selector de um Deployment é imutável, o `apply` falha.
9. `oc create deployment relatorios --image registry.exemplo.com/relatorios:1.0 --dry-run=client -o yaml --save-config > relatorios.yaml`
10. `oc create -R -f ~/app/`
11. `oc apply -f ~/app/ --dry-run=server --validate=true` (acrescente `-R` se houver subdiretórios)
12. `oc diff -f ~/app/`
13. `oc rollout restart deployment/relatorios`
14. `oc patch deployment relatorios -p '{"spec":{"template":{"spec":{"containers":[{"name":"app","resources":{"requests":{"cpu":"200m"}}}]}}}}'` (ou `oc set resources deployment/relatorios -c app --requests cpu=200m`)
15. `oc kustomize overlays/homologacao`
16. `oc apply -k overlays/homologacao` e `oc delete -k overlays/homologacao`
17. `oc create` não atualiza objetos existentes. Use `oc apply -f app/` (ele grava a anotação na primeira vez, se faltar).
18. Não é erro: é o comportamento dos generators. O antigo fica órfão; ele pode ser apagado depois de confirmar que nada (nem um rollback) depende dele.
19. `commonLabels` adiciona `tier: web` ao `spec.selector`, que é imutável em Deployments. Troque por `labels: - pairs: {tier: web}` (sem `includeSelectors`) ou, se o selector precisar mudar mesmo, recrie o Deployment de forma planejada.
20. Compare `oc get route <r> -o jsonpath='{.spec.to.name}'` com `oc get svc`: o Service ganhou o sufixo, a referência da Route não. Corrija com um patch na Route (ou configuração de `nameReference`).
21. A variável foi adicionada por fora (ex.: `oc set env`) e nunca esteve na `last-applied-configuration`, então o apply não a considera sua. Confirme com `oc apply view-last-applied deployment/<d>`; remova uma vez com `oc set env deployment/<d> MODO_DEBUG-`.
22. `volumeMounts` é mesclado pela chave `mountPath`; um caminho diferente vira um **novo** item. Use JSON Patch (`replace` no índice do mount) ou corrija o manifesto completo e aplique.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 2 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
