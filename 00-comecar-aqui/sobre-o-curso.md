# Sobre o curso DO280

| Campo | Valor |
|-------|-------|
| Curso | Red Hat OpenShift Administration II: Configuring a Production Cluster (DO280) |
| Versão | Red Hat OpenShift Container Platform **4.22** |
| Material | `DO280-RHOCP4.22-en-1-20260827` · edição 1 · 2026-08-27 · 468 páginas |
| Referência | Capítulo 0 — *Introduction*, PDF p. 1–14 (sumário p. 3–5; curso p. 8) |

## Para quem é

O DO280 forma o administrador que **configura** um cluster de produção usado por
várias equipes. O DO180 ensinou a **operar** aplicações em um cluster existente;
o DO280 trata do que o administrador da plataforma decide e controla:

- quem entra no cluster e o que cada um pode fazer (autenticação e RBAC);
- como o tráfego é protegido (TLS, NetworkPolicies) e exposto além do HTTP;
- quanto cada equipe pode consumir (quotas, limit ranges) e como ela se serve sozinha;
- como instalar e atualizar complementos (Operators) e o próprio cluster;
- como aplicações que precisam de privilégios especiais rodam com segurança.

O curso declara que essas habilidades valem para todas as variantes do OpenShift
(OpenShift Container Platform, ROSA e Azure Red Hat OpenShift).

Público citado: administradores de sistemas, SREs e arquitetos que precisam
entender a segurança de um cluster OpenShift.

## Objetivos oficiais (em linguagem própria)

| # | Objetivo | Capítulos |
|---|----------|-----------|
| 1 | Implantar aplicações de forma declarativa, com customizações por ambiente (manifestos, templates, Helm, Kustomize) | 1, 2 |
| 2 | Manter segurança e confiabilidade em clusters compartilhados por várias aplicações e equipes | 3, 6, 8 |
| 3 | Configurar autenticação, autorização e quotas de recursos | 3, 6 |
| 4 | Proteger o tráfego com NetworkPolicies e TLS | 4 |
| 5 | Expor aplicações com protocolos além de HTTP/TLS e conectar Pods a redes adicionais | 5 |
| 6 | Gerenciar atualizações do cluster e dos Operators | 7, 9 |

O curso afirma que DO180 + DO280 preparam para o exame **EX280**. A relação com os
objetivos do exame está em [certificacao/](../certificacao/README.md).

## Pré-requisitos declarados

- **DO180 v4.22** ou experiência equivalente com a console e a CLI do OpenShift.
  Este repositório se apoia no
  [guia de estudos do DO180](https://github.com/eduardoandradej/openshift-do180-4.22-estudos);
  veja o mapa em [revisao-do180.md](revisao-do180.md).
- Bash em nível intermediário: processamento de texto, edição, utilitários de rede e
  de segurança do Linux. A Red Hat recomenda o RH124 antes do DO280.

## Estrutura oficial do curso

| Cap. | Título original | Pasta |
|------|-----------------|-------|
| 1 | Declarative Resource Management | [01-gerenciamento-declarativo](../capitulos/01-gerenciamento-declarativo/README.md) |
| 2 | Deploying Packaged Applications | [02-aplicacoes-empacotadas](../capitulos/02-aplicacoes-empacotadas/README.md) |
| 3 | Authentication and Authorization | [03-autenticacao-autorizacao](../capitulos/03-autenticacao-autorizacao/README.md) |
| 4 | Network Security | [04-seguranca-de-rede](../capitulos/04-seguranca-de-rede/README.md) |
| 5 | Exposing Non-HTTP/SNI Applications | [05-redes-nao-http](../capitulos/05-redes-nao-http/README.md) |
| 6 | Enabling Developer Self-service | [06-autosservico](../capitulos/06-autosservico/README.md) |
| 7 | Managing Kubernetes Operators | [07-operadores-kubernetes](../capitulos/07-operadores-kubernetes/README.md) |
| 8 | Application Security | [08-seguranca-aplicacoes](../capitulos/08-seguranca-aplicacoes/README.md) |
| 9 | OpenShift Updates | [09-atualizacoes-openshift](../capitulos/09-atualizacoes-openshift/README.md) |
| 10 | Comprehensive Review | [10-revisao-integrada](../capitulos/10-revisao-integrada/README.md) |

As seções de cada capítulo (conforme o sumário do PDF) estão listadas no README de
cada pasta, mesmo antes de o capítulo ser desenvolvido.

## Convenções da apostila

A apostila usa quatro tipos de aviso: **References** (documentação externa),
**Note** (dica; ignorar não causa dano), **Important** (detalhe fácil de perder,
como algo que só vale na sessão atual) e **Warning** (ignorar pode causar perda de
dados). Aqui eles viram ⚠️ (alerta operacional) e 📘 (fonte externa).

---
[← Comece aqui](README.md) | [Índice](README.md) | [Próximo: Como usar este repositório →](como-usar-este-repositorio.md)
