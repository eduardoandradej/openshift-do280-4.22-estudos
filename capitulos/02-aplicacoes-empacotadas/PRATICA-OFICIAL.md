# Prática oficial — Capítulo 2

> Ponte para as atividades do curso. **Não** reproduz enunciados, passos, valores
> ou soluções. Execute tudo no ambiente oficial, no `workstation`, como `student`.
> As credenciais, URLs, versões e nomes estão no enunciado de cada atividade.
> Nenhum comando `lab` é executado por este repositório.

## Visão geral

| Seção | Tipo | Identificador `lab` | `lab grade` | PDF |
|-------|------|---------------------|-------------|-----|
| 2.2 | Guided Exercise | `packaged-templates` | Não | p. 76–81 |
| 2.4 | Guided Exercise | `packaged-charts` | Não | p. 94–98 |
| 2.5 | Lab | `packaged-review` | **Sim** | p. 99–108 |

Identificadores e avaliação verificados no PDF do Capítulo 2. O capítulo não tem quiz.

> As atividades com Helm usam um **repositório de charts da sala de aula** e imagens
> do **registry interno**: o ambiente não tem internet, então os valores padrão dos
> charts (que apontam para registries públicos) precisam ser sobrescritos.

---

## 2.2 Guided Exercise: Use OpenShift Templates

| Campo | Valor |
|-------|-------|
| Seção | 2.2 |
| Tipo | Guided Exercise |
| Identificador `lab` | `packaged-templates` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 76–81 |

**Competência praticada:** escolher um template do catálogo `openshift` para um banco
de dados e instanciá-lo; carregar um template próprio no projeto; processá-lo e
aplicá-lo com parâmetros coerentes com o banco; e atualizar a aplicação para uma
versão nova com um arquivo de parâmetros, **sem perder dados**.

**Domine antes:** [2.1](2.1-templates-do-openshift.md) (descrever templates, `oc new-app
--template`, `oc process | oc apply -f -`, `--param-file`, `oc diff -f -`).

```bash
lab start packaged-templates
lab finish packaged-templates
```

**Pontos de atenção:**
- Antes de instanciar, leia `Objects` e os parâmetros (quais são gerados?).
- Anote os valores que a aplicação e o banco precisam **compartilhar**; o template da aplicação também declara um objeto com o mesmo nome de um objeto do banco.
- Na atualização, observe no `oc diff` quais variáveis mudam por causa de parâmetros **omitidos** e se isso é desejado.
- Um Pod temporário (`oc run ... --rm`) é usado para testar o banco: repare como ele é removido ao final.

**Verificação conceitual:** banco e aplicação `Running`; a aplicação mostra a versão
nova após a atualização e os dados inseridos antes continuam lá.

**Diagnóstico se algo falhar:** `oc logs` da aplicação (erro de autenticação?) →
`oc process ... | oc diff -f -` (o que mudaria?) → conferir os valores do Secret com
`oc extract`.

---

## 2.4 Guided Exercise: Deploy Helm Charts

| Campo | Valor |
|-------|-------|
| Seção | 2.4 |
| Tipo | Guided Exercise |
| Identificador `lab` | `packaged-charts` |
| Avaliação | Sem `lab grade` |
| Referência | PDF p. 94–98 |

**Competência praticada:** adicionar o repositório de charts da sala de aula,
inspecionar versões e valores, instalar uma versão específica com imagem do registry
interno e host de Route próprio, fazer upgrade de versão, criar uma segunda release em
outro projeto e escalar essa release alterando valores.

**Domine antes:** [2.3](2.3-charts-do-helm.md) (`helm repo`, `helm search repo
--versions`, `helm show values`, `helm install/upgrade/list`).

```bash
lab start packaged-charts
lab finish packaged-charts
```

**Pontos de atenção:**
- Verifique se o repositório já está configurado antes de adicioná-lo.
- No `helm show values`, identifique como o chart monta a referência da imagem (mais de uma chave).
- Use sempre `--version` ao instalar e atualizar; confira a coluna CHART do `helm list`.
- Confira em qual projeto você está antes de cada `helm install`: a release vai para o namespace atual.
- A Route do chart usa terminação TLS *edge* (assunto do Capítulo 4): acesse por HTTPS.

**Verificação conceitual:** `helm list` mostra cada release na versão esperada; as
Routes respondem nos hosts definidos; a release escalada tem o número de Pods pedido.

**Diagnóstico se algo falhar:** `oc get pods` (`ImagePullBackOff` → valores de imagem)
→ `oc get route` (host) → `helm get values <release>` → `helm history <release>`.

---

## 2.5 Lab: Deploy Packaged Applications

| Campo | Valor |
|-------|-------|
| Seção | 2.5 |
| Tipo | Lab de fim de capítulo |
| Identificador `lab` | `packaged-review` |
| Avaliação | **`lab grade packaged-review`** |
| Referência | PDF p. 99–108 (enunciado p. 99–100; solução a partir da p. 101) |

**Competência praticada (resumo autoral):** com o repositório de charts da sala de
aula, criar dois ambientes (teste e produção) em projetos distintos, cada um com sua
release e seu arquivo de valores; usar imagem do registry privado; atualizar versões do
chart e da imagem; e adicionar requests e limits de memória pela configuração do chart.

**Domine antes:** a lição [2.3](2.3-charts-do-helm.md), o GE 2.4 e a [revisão](REVISAO.md).

```bash
lab start packaged-review
lab grade packaged-review
lab finish packaged-review
```

**Pontos de atenção:**
- Nomes de projetos, releases, arquivos de valores, versões do chart, tags de imagem e host são conferidos pelo `lab grade`: use exatamente os do enunciado.
- Mantenha **um arquivo de valores por ambiente** e passe-o em **todo** `install`/`upgrade` daquele ambiente.
- O próprio `values.yaml` do chart traz, em comentários, o formato do bloco de recursos.
- Antes de cada comando `helm`, confirme o namespace (`oc project` ou `-n`).

**Verificação conceitual:** `helm list -A` mostra as duas releases nas versões
esperadas; `oc describe pod` do ambiente de produção mostra os requests/limits; o de
teste não; a Route de produção responde no host definido.

**Diagnóstico se algo falhar:** saída do `lab grade` → `helm list -A` → `helm get
values <release> -n <ns>` → `oc describe pod -n <ns>`.

---

**Registro:** anote cada atividade no [diário de laboratórios](../../progresso/diario-de-laboratorios.md).

---
[← 2.3 Charts do Helm](2.3-charts-do-helm.md) | [Índice do capítulo](README.md) | [Próximo: Revisão →](REVISAO.md)
