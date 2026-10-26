# Objetivos de aprendizagem

| Marca | Significado |
|-------|-------------|
| ✅ | Confirmado no PDF do curso |

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

## 7. Capítulo 6 — Enabling Developer Self-service (✅ PDF p. 245–302)

Meta do capítulo: configurar clusters para que desenvolvedores criem projetos com limites de uso de recursos.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Configurar quotas de computação e de contagem de objetos por projeto e para vários projetos | [6.1](../capitulos/06-autosservico/6.1-quotas-de-projeto-e-cluster.md) | [ ] |
| Configurar requisitos padrão e máximos de recursos dos Pods de cada projeto | [6.3](../capitulos/06-autosservico/6.3-limit-ranges.md) | [ ] |
| Configurar restrições padrão para projetos novos e gerenciar quem pode criá-los | [6.5](../capitulos/06-autosservico/6.5-template-de-projeto-e-self-provisioner.md) | [ ] |

## 8. Capítulo 7 — Managing Kubernetes Operators (✅ PDF p. 303–350)

Meta do capítulo: instalar e atualizar operators gerenciados pelo OLM e pelo CVO.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Descrever o padrão operator e as formas de instalar e atualizar operators | [7.1](../capitulos/07-operadores-kubernetes/7.1-operators-e-olm.md) | [ ] |
| Instalar e atualizar operators pela web console e pelo OLM | [7.3](../capitulos/07-operadores-kubernetes/7.3-instalar-operators-pela-console.md) | [ ] |
| Instalar e atualizar operators usando as APIs do OLM | [7.5](../capitulos/07-operadores-kubernetes/7.5-instalar-operators-pela-cli.md) | [ ] |

## 9. Capítulo 8 — Application Security (✅ PDF p. 351–393)

Meta do capítulo: executar aplicações que precisam de privilégios elevados ou especiais do host ou do Kubernetes.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Criar service accounts, aplicar permissões e gerenciar SCCs | [8.1](../capitulos/08-seguranca-aplicacoes/8.1-security-context-constraints.md) | [ ] |
| Permitir que uma aplicação acesse as APIs do Kubernetes do cluster | [8.3](../capitulos/08-seguranca-aplicacoes/8.3-acesso-de-aplicacoes-a-api.md) | [ ] |
| Automatizar tarefas regulares de gerenciamento com cron jobs | [8.5](../capitulos/08-seguranca-aplicacoes/8.5-manutencao-com-cron-jobs.md) | [ ] |

## 10. Capítulo 9 — OpenShift Updates (✅ PDF p. 394–433)

Meta do capítulo: atualizar um cluster OpenShift minimizando a interrupção das aplicações.

| Objetivo da seção | Lição | Estudado? |
|-------------------|-------|-----------|
| Descrever o processo de atualização do cluster | [9.1](../capitulos/09-atualizacoes-openshift/9.1-atualizar-clusters.md) | [ ] |
| Identificar aplicações que usam APIs obsoletas do Kubernetes | [9.3](../capitulos/09-atualizacoes-openshift/9.3-apis-obsoletas.md) | [ ] |
| Atualizar operators gerenciados pelo OLM pela console e pela CLI | [9.5](../capitulos/09-atualizacoes-openshift/9.5-atualizar-operators-com-olm.md) | [ ] |

## 11. Capítulo 10 — Comprehensive Review (✅ PDF p. 434–468)

Meta do capítulo: validar o conhecimento adquirido no DO280 (apoio à preparação para o EX280).

| Objetivo | Material | Estudado? |
|----------|----------|-----------|
| Revisar de forma integrada os objetivos dos capítulos 1 a 9 | [10.1](../capitulos/10-revisao-integrada/10.1-resumo-dos-objetivos-de-aprendizagem.md) | [ ] |
| Configurar o autosserviço do cluster por especificação | [Lab 10.2](../capitulos/10-revisao-integrada/PRATICA-OFICIAL.md#102-lab-set-up-cluster-self-service) | [ ] |
| Proteger aplicações (TLS, network policies, manutenção) por especificação | [Lab 10.3](../capitulos/10-revisao-integrada/PRATICA-OFICIAL.md#103-lab-secure-applications) | [ ] |
| Implantar aplicações empacotadas (Helm, template, Kustomize) por especificação | [Lab 10.4](../capitulos/10-revisao-integrada/PRATICA-OFICIAL.md#104-lab-deploy-packaged-applications) | [ ] |

---
[Início](../README.md) | [Estado do projeto](estado-do-projeto.md)
