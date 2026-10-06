---
title: Métricas
---

O framework propõe duas métricas principais e complementares, mais um indicador auxiliar.

## Cobertura Arquitetural

Visão **estática**. Responde: *qual proporção dos módulos relevantes/críticos possui justificativa arquitetural vigente?*

$$
CA = \frac{|M_{cobertos}|}{|M_{criticos}|} \times 100
$$

- $M_{criticos}$: módulos classificados como críticos.
- $M_{cobertos}$: módulos críticos vinculados a **pelo menos uma ADR vigente**.

Varia de 0% a 100%. Valores altos indicam mais módulos críticos com decisão vigente associada.

### Módulo crítico

Para não depender de julgamento subjetivo, o conceito exige **critério operacional explícito**. Candidatos:

- complexidade ciclomática acima de um limiar;
- quantidade de módulos dependentes;
- quantidade de dependências;
- centralidade estrutural;
- combinação de duas ou mais características.

Sem integração com código-fonte, os valores são **informados manualmente** no cadastro.

> [!note]
> O critério de criticidade deve ser definido **antes** da avaliação experimental.

## Impacto de Decisão

Visão **dinâmica**. Mede quanto do escopo arquitetural pré-existente é potencialmente afetado por uma nova ADR.

$$
ID = \frac{|M_{afetados}|}{|M_{escopo\ anterior}|} \times 100
$$

- $M_{afetados}$: módulos potencialmente afetados pela nova decisão.
- $M_{escopo\ anterior}$: módulos associados às decisões existentes **antes** da nova ADR.

### Quando um módulo é afetado

**Condição A — substituição explícita.** O módulo está associado a uma ADR explicitamente substituída pela nova (`supersedes` / `superseded-by`).

**Condição B — sobreposição automática de escopo.** Há interseção entre componentes declarados na nova ADR e os de decisões anteriores. Forma inicial:

$$
|Componentes_{nova} \cap Componentes_{anterior}| \geq 1
$$

> [!important]
> O limiar de sobreposição deve ser definido operacionalmente **antes** da avaliação e mantido constante nos testes.

## Classificação dos módulos afetados

### Recoberto

Módulo que (1) foi afetado por substituição ou sobreposição e (2) está **explicitamente incluído no escopo de uma nova ADR vigente**. A decisão antiga caiu, mas há decisão vigente que o justifica.

### Órfão Arquitetural

Módulo que (1) foi afetado por ADR que deixou de ser vigente e (2) não está coberto por nenhuma outra ADR vigente após a alteração.

$$
Orfao(m) = Afetado(m) \land \neg ExisteADRValida(m)
$$

### Taxa de Orfandade Arquitetural

Indicador complementar para o relatório de governança:

$$
TO = \frac{|M_{orfaos}|}{|M_{afetados}|} \times 100
$$

## Questões em aberto

> [!question] Módulo afetado que não é nem Recoberto nem Órfão
> Um módulo afetado pela **Condição B**, cuja ADR antiga **continua vigente** e que **não** está no escopo da nova ADR, não é Recoberto (não está na nova ADR) nem Órfão (ainda existe ADR vigente). A taxonomia tem um buraco. Opções: terceira classe (ex.: *sobreposição / conflito potencial*) ou restringir a Condição B.

> [!question] Sobreposição com ADRs não vigentes
> A Condição B deve considerar só ADRs anteriores **vigentes**? Sobreposição com ADR já substituída ou depreciada não deveria contar como impacto novo.

> [!question] Denominadores diferentes
> A Cobertura usa apenas módulos **críticos**; o Impacto usa **todos** os módulos do escopo anterior. Decidir se é intencional ou se o Impacto também se restringe a críticos.

> [!question] Limiar de sobreposição
> "≥ 1 componente em comum" tende a gerar falsos positivos (ADRs complementares que compartilham um componente sem conflito). Alternativas: proporção de sobreposição (ex.: Jaccard), peso por tipo de relação, domínio da ADR. Ver [[06-avaliacao#Variantes da regra|variantes da regra]].

> [!question] Critério de criticidade
> Escolher e fundamentar o critério (e os limiares) de módulo crítico.

## Exemplo numérico (a elaborar)

Um exemplo trabalhado com 6 a 8 módulos, 3 a 4 ADRs e uma nova ADR, mostrando CA antes/depois, ID e a classificação Recoberto/Órfão, será adicionado aqui para validar as definições.
