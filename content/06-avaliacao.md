---
title: Estratégia de avaliação
---

Dois estudos **independentes**: um mede percepção, o outro mede capacidade de detecção.

## Estudo 1 — Aceitação da ferramenta (TAM)

Avalia o **Technology Acceptance Model**:

- utilidade percebida;
- facilidade de uso percebida;
- intenção de utilização.

> [!note]
> Se for preciso avaliar a **usabilidade da interface**, usar também o **System Usability Scale (SUS)**, mantendo o TAM para aceitação.

**A definir:** número e perfil dos participantes, tarefa realizada, instrumento (itens, escala Likert), forma de análise.

## Estudo 2 — Detecção de impacto

Compara o resultado automático com a análise manual de especialistas, **sem** misturar com a percepção de uso.

**Cenários** contendo: ADRs vigentes e substituídas, relações `supersedes`, módulos/componentes associados e uma nova ADR.

**Procedimento:** especialistas indicam quais módulos consideram afetados; o resultado é comparado com o do VAL-TRACE.

### Matriz de confusão

| | Especialista: afetado | Especialista: não afetado |
|---|---|---|
| **VAL-TRACE: afetado** | Verdadeiro Positivo (VP) | Falso Positivo (FP) |
| **VAL-TRACE: não afetado** | Falso Negativo (FN) | Verdadeiro Negativo (VN) |

$$ Precisao = \frac{VP}{VP + FP} \qquad Revocacao = \frac{VP}{VP + FN} $$

$$ F1 = 2 \times \frac{Precisao \times Revocacao}{Precisao + Revocacao} $$

Com mais de um especialista, medir a **concordância entre avaliadores** (ex.: Cohen/Fleiss κ) antes de fixar o conjunto de referência.

## Variantes da regra

Para mostrar o ganho real da sobreposição automática, comparar:

1. **Baseline:** só Condição A (`supersedes` manual).
2. **A + B (≥ 1 componente em comum).**
3. **A + B com proporção** (ex.: Jaccard acima de um limiar).

Cada variante tem seu P/R/F1. Ver [[03-metricas#Questões em aberto|questões da métrica]].

## Riscos e questões em aberto

> [!question] Circularidade
> Se o algoritmo só compara listas declaradas e os especialistas veem as mesmas listas, a concordância pode ser quase trivial. Os cenários precisam incluir impacto **semântico/indireto**, que o modelo atual não representa.

> [!question] Só "afetado" é avaliado
> A classificação **Recoberto/Órfão**, contribuição central, ficaria sem validação. Pedir também esse rótulo aos especialistas.

> [!question] Tamanhos amostrais e origem dos cenários
> Número de cenários, origem (projetos open-source com ADRs reais? sintéticos? ambos?), número e perfil de especialistas.
