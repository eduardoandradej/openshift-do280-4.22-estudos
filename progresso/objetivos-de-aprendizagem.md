# Objetivos de aprendizagem

| Marca | Significado |
|-------|-------------|
| ✅ | Confirmado no PDF do curso |
| ⏳ | Capítulo ainda não recebido |

A relação com o exame EX280 está em [certificacao/matriz-ex280.md](../certificacao/matriz-ex280.md).

## 1. Objetivos gerais do DO280 (✅ PDF p. 8)

| # | Objetivo (linguagem própria) | Capítulos | Estudado? |
|---|------------------------------|-----------|-----------|
| 1 | Implantar aplicações de forma declarativa, com customizações por ambiente (manifestos, templates, Helm, Kustomize) | 1, 2 | [ ] |
| 2 | Manter segurança e confiabilidade em clusters com várias aplicações e equipes | 3, 6, 8 | [ ] |
| 3 | Configurar autenticação, autorização e quotas | 3, 6 | [ ] |
| 4 | Proteger o tráfego com NetworkPolicies e TLS | 4 | [ ] |
| 5 | Expor aplicações com protocolos além de HTTP/TLS e usar redes adicionais | 5 | [ ] |
| 6 | Gerenciar atualizações do cluster e de Operators | 7, 9 | [ ] |

## 2. Capítulo 1 — Declarative Resource Management (✅ PDF p. 15–68)

Meta do capítulo: implantar e atualizar aplicações a partir de manifestos
parametrizados para diferentes ambientes.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Implantar e atualizar aplicações a partir de manifestos YAML | [1.1](../capitulos/01-gerenciamento-declarativo/1.1-manifestos-de-recursos.md) | [ ] |
| Implantar e atualizar aplicações a partir de manifestos complementados pelo Kustomize | [1.3](../capitulos/01-gerenciamento-declarativo/1.3-overlays-do-kustomize.md) | [ ] |

## 3. Capítulo 2 — Deploying Packaged Applications (✅ PDF p. 69–109)

Meta do capítulo: implantar e atualizar aplicações a partir de manifestos empacotados
para compartilhamento e distribuição.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Implantar e atualizar aplicações a partir de manifestos empacotados como templates do OpenShift | [2.1](../capitulos/02-aplicacoes-empacotadas/2.1-templates-do-openshift.md) | [ ] |
| Implantar e atualizar aplicações a partir de manifestos empacotados como charts do Helm | [2.3](../capitulos/02-aplicacoes-empacotadas/2.3-charts-do-helm.md) | [ ] |

## 4. Capítulo 3 — Authentication and Authorization (✅ PDF p. 110–145)

Meta do capítulo: configurar a autenticação com o provedor HTPasswd e atribuir papéis
a usuários e grupos.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Explicar provedores de identidade e configurar o provedor HTPasswd | [3.1](../capitulos/03-autenticacao-autorizacao/3.1-provedor-de-identidade-htpasswd.md) | [ ] |
| Definir controles de acesso baseados em papéis e aplicar permissões a usuários | [3.3](../capitulos/03-autenticacao-autorizacao/3.3-permissoes-com-rbac.md) | [ ] |

## 5. Capítulo 4 — Network Security (✅ PDF p. 146–198)

Meta do capítulo: proteger o tráfego de rede entre aplicações dentro e fora do cluster.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Permitir e proteger conexões de rede às aplicações dentro do cluster | [4.1](../capitulos/04-seguranca-de-rede/4.1-tls-no-trafego-externo.md) | [ ] |
| Restringir o tráfego de rede entre projetos e Pods | [4.3](../capitulos/04-seguranca-de-rede/4.3-network-policies.md) | [ ] |
| Configurar e usar certificados de serviço automáticos | [4.5](../capitulos/04-seguranca-de-rede/4.5-tls-no-trafego-interno.md) | [ ] |

## 6. Capítulo 5 — Exposing Non-HTTP/SNI Applications (✅ PDF p. 199–244)

Meta do capítulo: expor aplicações ao acesso externo sem usar um ingress controller.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Expor aplicações ao acesso externo com Services LoadBalancer | [5.1](../capitulos/05-redes-nao-http/5.1-servicos-load-balancer.md) | [ ] |
| Configurar UDNs para isolamento e NADs para acesso externo | [5.3](../capitulos/05-redes-nao-http/5.3-redes-adicionais-de-pods.md) | [ ] |

## 7. Capítulos 6 a 10

⏳ Os objetivos de cada seção serão registrados aqui quando os PDFs forem recebidos.

---
[Início](../README.md) | [Estado do projeto](estado-do-projeto.md)
