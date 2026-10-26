# Prática oficial — Capítulo 1

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`.
> As credenciais e URLs estão no enunciado de cada atividade. Nenhum comando `lab`
> é executado por este repositório.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | PDF |
|-------|------|---------------------|-------------|-----|
| 1.2 | Guided Exercise | `declarative-manifests` | Não | p. 26–34 |
| 1.4 | Guided Exercise | `declarative-kustomize` | Não | p. 47–57 |
| 1.5 | Lab | `declarative-review` | **Sim** | p. 58–67 |

Identificadores e avaliação verificados no PDF do Capítulo 1. O capítulo não tem quiz.

> As três atividades usam repositórios no **GitLab interno** (`git.lab.example.com`)
> em que **branches Git representam versões** da aplicação. Antes de começar, tenha
> clareza de como ver as branches remotas (`git branch -a`, `git log --oneline`),
> trocar de versão (`git checkout <branch>`) e ver o que mudou (`git show`).

---

## 1.2 Guided Exercise: Resource Manifests

| Campo | Valor |
|-------|-------|
| Seção | 1.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `declarative-manifests` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 26–34 |

**Competência praticada:** implantar uma aplicação web com banco de dados a partir de
manifestos YAML guardados no Git, avançar por versões sucessivas, inspecionar as
mudanças **antes** de aplicá-las e forçar a recriação de Pods quando a mudança de
configuração não gera rollout sozinha.

**Domine antes:** [1.1](1.1-manifestos-de-recursos.md) (Partes A e C: `--dry-run=server`,
`oc diff`, `oc apply`, `oc rollout restart`, `last-applied-configuration`).

```bash
lab start declarative-manifests
lab finish declarative-manifests
```

**Pontos de atenção:**
- Valide o diretório inteiro com `--dry-run=server` antes do primeiro `apply` e leia o que seria criado.
- A cada nova versão, leia o `oc diff` **e** responda: qual Deployment terá Pods novos? Qual objeto mudou sem afetar Pods?
- Um `CrashLoopBackOff` temporário na primeira implantação pode ser só a aplicação subindo antes do banco; distinga isso de uma falha persistente.
- Observe que o `oc diff` mascara valores de Secrets.
- Na última versão, investigue **qual componente** ficou com configuração antiga antes de reiniciar algo.

**Verificação conceitual:** todos os Pods `Running` e `1/1`; a versão esperada da
aplicação visível pela Route; a anotação `last-applied-configuration` presente nos
Deployments.

**Diagnóstico se algo falhar:** `oc get pods` → `oc logs <pod>` (erro de conexão ou
autenticação com o banco?) → `oc diff -f .` (falta aplicar algo?) → idade dos Pods de
cada Deployment (quem não foi recriado?).

---

## 1.4 Guided Exercise: Kustomize Overlays

| Campo | Valor |
|-------|-------|
| Seção | 1.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `declarative-kustomize` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 47–57 |

**Competência praticada:** implantar com `oc apply -k` uma aplicação cuja base é
composta por outras bases e usa generators de ConfigMap e Secret; aplicar versões
novas (mudança de imagem e mudança de credencial) e observar o efeito do hash dos
generators; aplicar um overlay de produção que altera o número de réplicas.

**Domine antes:** [1.3](1.3-overlays-do-kustomize.md) (base × overlay, `patches` +
`target`, generators e hash, `oc apply -k`, `oc delete -k`).

```bash
lab start declarative-kustomize
lab finish declarative-kustomize
```

**Pontos de atenção:**
- Leia o `kustomization.yaml` da base raiz e identifique: quais bases ela compõe, o que é gerado e quem consome cada objeto gerado.
- Use `oc kustomize <dir>` para ver o resultado antes de aplicar.
- Depois de mudar um valor do `secretGenerator`, compare `oc get secret` antes e depois: o nome muda e o antigo continua lá.
- Confira para qual Secret o Deployment aponta (JSONPath em `envFrom`).
- Observe que recursos sem mudança não são reiniciados.
- Repare que o warning sobre `DeploymentConfig` exibido por `oc get all` é apenas informativo (API obsoleta desde 4.14).

**Verificação conceitual:** a URL da aplicação mostra a versão esperada após cada
etapa; o Deployment referencia o Secret com o hash mais recente; o overlay de produção
resulta em duas réplicas.

**Diagnóstico se algo falhar:** `oc kustomize` (o YAML renderizado está como você
esperava?) → `oc get deployment -o jsonpath` nas referências → `oc get pods` e logs.

---

## 1.5 Lab: Manage Declarative Resources

| Campo | Valor |
|-------|-------|
| Seção | 1.5 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `declarative-review` |
| Avaliação | **`lab grade declarative-review`** |
| Referência | PDF p. 58–67 (enunciado p. 58–59; solução a partir da p. 60) |

**Competência praticada (resumo autoral):** a partir de um repositório Git com base e
overlay Kustomize, implantar uma versão da aplicação em um projeto, atualizá-la para a
versão seguinte e implantar o overlay de produção em um **segundo projeto**,
confirmando versão e número de réplicas pelas Routes.

**Domine antes:** as duas lições, o GE 1.4 e a [revisão](REVISAO.md).

```bash
lab start declarative-review
lab grade declarative-review
lab finish declarative-review
```

**Pontos de atenção:**
- Os **nomes dos projetos** e a **branch** de cada etapa são conferidos pelo `lab grade`; use exatamente os do enunciado.
- Antes de aplicar o overlay, confira se ele define `namespace` e em qual projeto você está; o Kustomize não cria projetos.
- Use `oc kustomize` para prever o número de réplicas e o que muda entre base e overlay.
- Verifique cada etapa pela Route correspondente a cada projeto.

**Verificação conceitual:** dois projetos com a aplicação funcionando; o de produção
com o número de réplicas definido pelo overlay; ambos na versão pedida.

**Diagnóstico se algo falhar:** releia a saída do `lab grade` (cada critério indica o
que falta) → `oc get all -n <projeto>` → `oc kustomize` → `git branch` (versão certa?).

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 1.3 Overlays do Kustomize](1.3-overlays-do-kustomize.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
