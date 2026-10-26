# Revisão — Capítulo 2

Questões **autorais**. Elas não reproduzem atividades oficiais nem perguntas de exame.
Responda sem consultar as lições; confira no final.

## Parte 1 — Conceitos

1. Quais são as partes principais de um objeto `Template` e o que cada uma faz?
2. Por que "processar" um template não cria nada no cluster? Quais são as duas formas de levar o resultado ao cluster?
3. Que risco existe ao reprocessar e reaplicar um template que tem parâmetros com `generate: expression`?
4. Explique a diferença entre chart, versão do chart, release e revisão.
5. Onde o Helm guarda o histórico de uma release e quem tem acesso a ele?
6. Por que `helm repo add` não altera nada no cluster?
7. Em que situação um Operator é preferível a um chart do Helm?
8. Compare, em uma frase cada, Kustomize, templates e Helm quanto a **quem decide o que pode variar**.

## Parte 2 — Comandos (escreva o comando)

9. Listar os templates disponíveis no catálogo global do cluster.
10. Mostrar os parâmetros do template `httpd-example` do namespace `openshift`.
11. Processar o template do arquivo `loja.yaml` com os parâmetros de `loja-prod.txt` e mostrar o que mudaria no cluster, sem aplicar.
12. Carregar o template definido em `loja.yaml` no projeto `modelos-equipe`.
13. Listar todas as versões do chart `web` do repositório `corp`.
14. Instalar a versão `1.4.0` do chart `corp/web` como release `hml` no projeto `web-hml`, com os valores de `values-hml.yaml`, apenas pré-visualizando.
15. Atualizar a release `hml` para a versão `1.5.0` mantendo os valores do ambiente.
16. Mostrar o histórico da release `hml` e voltar para a revisão 1.
17. Ver os valores finais (padrões + sobrescritas) da release `hml`.

## Parte 3 — Diagnóstico

18. Um usuário comum recebe erro de permissão ao executar `oc process mysql-persistent -n openshift`. Como ele obtém os manifestos?
19. Após `helm install`, os Pods ficam em `ImagePullBackOff` no ambiente sem acesso à internet. Onde você procura e o que corrige?
20. Depois de `helm upgrade app repo/app --set replicaCount=4 --version 2.0.0`, a Route voltou ao host padrão. Explique e corrija o processo.
21. `helm list` não mostra a release que você instalou ontem. Cite duas causas possíveis.
22. Uma equipe instalou um chart por URL na console e agora não consegue escolher outra versão do chart no upgrade. Por quê?

---

<details>
<summary>Respostas comentadas</summary>

1. `objects` (manifestos a gerar), `parameters` (lacunas, com padrão, obrigatoriedade ou gerador), `labels` (aplicados a todos os objetos), `message` (texto pós-instanciação) e `metadata` (nome, descrição, anotações).
2. Processar só substitui parâmetros e devolve uma lista de manifestos. Para criar: `oc new-app --template` (cria direto) ou `oc process ... | oc apply -f -` (cria ou atualiza).
3. O valor é gerado de novo a cada processamento; omitir o parâmetro na atualização muda, por exemplo, uma senha no Secret, quebrando a aplicação.
4. Chart é o pacote; versão é cada edição publicada dele; release é uma instalação nomeada em um namespace; revisão é cada estado da release (install, upgrade, rollback).
5. Em Secrets do tipo `helm.sh/release.v1` no namespace da release; quem pode ler Secrets daquele namespace.
6. Porque a lista de repositórios é configuração local do cliente (`~/.config/helm/repositories.yaml`).
7. Quando a aplicação exige gestão contínua de ciclo de vida (backups, failover, upgrades coordenados), que um controller executa melhor do que uma instalação pontual de manifestos.
8. Kustomize: quem **usa** altera qualquer campo por patch. Templates: o **autor** define parâmetros simples. Helm: o **autor** define valores, com lógica de template, versões e histórico.
9. `oc get templates -n openshift`
10. `oc process --parameters httpd-example -n openshift`
11. `oc process -f loja.yaml --param-file loja-prod.txt | oc diff -f -`
12. `oc create -f loja.yaml -n modelos-equipe`
13. `helm search repo corp/web --versions`
14. `helm install hml corp/web --version 1.4.0 -f values-hml.yaml -n web-hml --dry-run`
15. `helm upgrade hml corp/web --version 1.5.0 -f values-hml.yaml -n web-hml`
16. `helm history hml -n web-hml` e `helm rollback hml 1 -n web-hml`
17. `helm get values hml --all -n web-hml`
18. Copiando o template: `oc get template mysql-persistent -n openshift -o yaml > mysql.yaml` e processando a cópia (`oc process -f mysql.yaml ...`), ou carregando-a no próprio projeto.
19. `oc describe pod` mostra a imagem que o chart montou; `helm show values` mostra as chaves de imagem. Sobrescreva repositório/nome/tag para o registry interno no arquivo de valores e faça `helm upgrade`.
20. Ao informar valores novos, o Helm usa os padrões do chart + só os valores dados agora; o host passado antes não foi reaproveitado. Mantenha um arquivo de valores completo por ambiente e passe-o em todo upgrade.
21. Namespace diferente do atual (use `-n` ou `-A`); a release foi desinstalada ou instalada com outro nome; você está em outro cluster/contexto.
22. Pela console, releases instaladas por URL não permitem trocar a versão do chart no upgrade; só os valores.

</details>

## Autoavaliação

| Acertos | Ação |
|---------|------|
| 20–22 | Siga para o Capítulo 3 |
| 15–19 | Refaça a Parte C das lições em que errou |
| < 15 | Repita os guided exercises e registre em [dificuldades-identificadas.md](../../progresso/dificuldades-identificadas.md) |

---
[← Prática oficial](PRATICA-OFICIAL.md) | [Índice do capítulo](README.md) | [Próximo: Resumo →](RESUMO.md)
