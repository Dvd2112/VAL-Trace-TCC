---
title: VAL-TRACE
---

# VAL-TRACE

> **Um Framework e Ferramenta para Rastreabilidade e Análise de Impacto de Decisões Arquiteturais na Mitigação do Desvio Arquitetural e do Débito Técnico.**

Este é o documento principal do TCC. Resume o projeto inteiro e aponta, em cada seção, a nota onde o assunto é detalhado.

> [!warning] Documento em construção
> Várias definições ainda estão abertas. Elas aparecem como **Questão em aberto** nas notas e estão listadas em [[#Questões em aberto|Questões em aberto]].

---

## 1. Resumo

Decisões arquiteturais mudam ao longo da vida de um sistema, mas a ligação entre **requisitos**, **decisões registradas (ADRs)** e **módulos de código** raramente é mantida. Quando uma ADR é substituída, os módulos que ela cobria podem ficar sem justificativa arquitetural vigente: os **órfãos arquiteturais**. Isso favorece o *architectural drift* e o débito técnico.

O **VAL-TRACE** propõe um **framework** de rastreabilidade (`Requisito → ADR → Módulo/Componente`) e uma **ferramenta web** que:

1. mede o estado atual da cobertura arquitetural (**Cobertura Arquitetural**);
2. mede o efeito de uma nova ADR sobre o escopo existente (**Impacto de Decisão**), combinando substituição explícita (`supersedes`) com detecção automática de sobreposição de escopo;
3. classifica os módulos afetados como **Recobertos** ou **Órfãos**.

A avaliação tem dois estudos separados: **aceitação** (TAM) e **acurácia da detecção de impacto** frente a especialistas.

Detalhes: [[01-contexto-problema]].

---

## 2. Problema e pergunta de pesquisa

### O problema em quatro passos

1. Requisitos motivam decisões, mas a ligação raramente é registrada além de texto livre.
2. ADRs documentam decisões, mas **registrar não é rastrear**.
3. Quando uma ADR é substituída, `supersedes`/`superseded-by` indicam a relação entre ADRs, **não o efeito sobre os módulos**.
4. Módulos ficam associados a decisões que já não valem: **órfãos arquiteturais**.

### Pergunta de pesquisa

> Como um framework e uma ferramenta de rastreabilidade, combinando relações explícitas entre ADRs e detecção automática de sobreposição de escopo, podem quantificar a cobertura arquitetural de um sistema e o impacto de novas decisões arquiteturais sobre módulos previamente existentes?

Detalhes: [[01-contexto-problema]].

---

## 3. Objetivos

**Geral.** Propor o framework VAL-TRACE e desenvolver uma ferramenta web que calcule Cobertura Arquitetural e Impacto de Decisão a partir de relações explícitas entre ADRs e da sobreposição automática de módulos/componentes declarados.

**Específicos.**

| # | Objetivo | Onde |
|---|---|---|
| 1 | Modelo de dados (requisitos, ADRs, módulos, relações) | [[02-modelo-conceitual]] |
| 2 | ADR estendida com `supersedes` / `superseded-by` | [[04-framework/artefatos]] |
| 3 | Vinculação requisito ↔ ADR ↔ módulo/componente | [[02-modelo-conceitual]] |
| 4 | Aplicação web de cadastro manual | [[07-ferramenta]] |
| 5 | Cálculo da Cobertura Arquitetural | [[03-metricas]] |
| 6 | Cálculo do Impacto de Decisão | [[03-metricas]] |
| 7 | Detecção automática de sobreposição de escopo | [[03-metricas]] |
| 8 | Classificação Recoberto / Órfão | [[03-metricas]] |
| 9 | Avaliação de aceitação (TAM) | [[06-avaliacao]] |
| 10 | Avaliação da detecção de impacto vs. especialistas | [[06-avaliacao]] |

---

## 4. Delimitação

- Ferramenta **independente**, sem integração com Git, análise estática ou plataformas externas.
- Requisitos, ADRs, módulos e componentes são **cadastrados manualmente**.
- Motivo: concentrar a investigação na validade do modelo, das regras e das métricas, sem variáveis de integração tecnológica.
- Integração com repositórios e análise estática: **trabalho futuro**.
- O estudo **não** pretende provar a eliminação do drift ou do débito técnico, apenas se a rastreabilidade ajuda a **identificar antecipadamente** situações que os favorecem.

---

## 5. Modelo conceitual

```
Requisito → Decisão Arquitetural (ADR) → Módulo/Componente
```

Uma ADR pode atender a vários requisitos, abranger vários módulos/componentes, substituir uma ADR anterior e ser substituída por uma posterior. Estados previstos: *proposta, aceita, vigente, rejeitada, depreciada, substituída*.

Detalhes: [[02-modelo-conceitual]].

---

## 6. Métricas

| Métrica | Natureza | Pergunta que responde |
|---|---|---|
| **Cobertura Arquitetural (CA)** | Estática | Que proporção dos módulos críticos tem ADR vigente? |
| **Impacto de Decisão (ID)** | Dinâmica | Quanto do escopo anterior uma nova ADR afeta? |
| **Taxa de Orfandade (TO)** | Complementar | Dos módulos afetados, quantos ficaram sem cobertura? |

$$
CA = \frac{|M_{cobertos}|}{|M_{criticos}|} \times 100
\qquad
ID = \frac{|M_{afetados}|}{|M_{escopo\ anterior}|} \times 100
\qquad
TO = \frac{|M_{orfaos}|}{|M_{afetados}|} \times 100
$$

**Um módulo é afetado** quando:

- **Condição A:** está ligado a uma ADR explicitamente substituída pela nova (`supersedes`); ou
- **Condição B:** há sobreposição de componentes entre a nova ADR e as anteriores (forma inicial: $\geq 1$ componente em comum).

**Classificação dos afetados:**

- **Recoberto:** afetado, mas explicitamente incluído no escopo de uma nova ADR vigente.
- **Órfão:** afetado e sem nenhuma ADR vigente que o cubra: $Orfao(m) = Afetado(m) \land \neg ExisteADRValida(m)$.

**Módulo crítico:** exige critério operacional explícito (complexidade ciclomática, nº de dependentes/dependências, centralidade ou combinação), definido antes da avaliação e informado manualmente.

Detalhes e questões abertas: [[03-metricas]].

---

## 7. Framework

O VAL-TRACE usa três referências como apoio **complementar**, sem formar um novo framework arquitetural:

| Referência | Papel no VAL-TRACE |
|---|---|
| **C4 Model** | Estrutura o escopo técnico (contêiner e componente); é o grão da sobreposição |
| **Modelo 4+1** | Classifica o tipo de impacto; cenários como validação; fora do cálculo |
| **IAF** | Domínio da ADR (aplicação, informação, infraestrutura, integração) e organização dos artefatos |

**Processo em quatro etapas:**

```
1. Requisitos e escopo → 2. Registro e vinculação de ADRs
        → 3. Análise de cobertura e impacto → 4. Diagnóstico e governança
```

**Artefatos:**

| Tipo | Artefato |
|---|---|
| Entrada | Especificação de Requisitos; Catálogo de Módulos e Componentes |
| Modelagem | **Ficha de ADR Expandida** |
| Saída | Matriz de Rastreabilidade Requisito–ADR–Módulo; Painel de Cobertura e Impacto |

Detalhes: [[04-framework/index|Framework]] · [[04-framework/fundamentacao]] · [[04-framework/processo]] · [[04-framework/artefatos]].

---

## 8. Metodologia

**Design Science Research**, em quatro fases:

| Fase | Entregável |
|---|---|
| 1. Identificação do problema | Fundamentação teórica e lacuna de pesquisa |
| 2. Concepção | Modelo conceitual do VAL-TRACE |
| 3. Desenvolvimento | Protótipo funcional |
| 4. Avaliação | Evidências quantitativas de aceitação e desempenho |

Detalhes: [[05-metodologia-dsr]].

---

## 9. Avaliação

Dois estudos **independentes**:

| Estudo | Mede | Instrumento / métricas |
|---|---|---|
| **1. Aceitação** | Percepção dos desenvolvedores | TAM (utilidade, facilidade, intenção); SUS opcional |
| **2. Detecção de impacto** | Acurácia do resultado automático frente a especialistas | Matriz de confusão; Precisão, Revocação e F1; concordância entre avaliadores |

Para isolar o ganho da sobreposição automática, as variantes da regra são comparadas: só Condição A (baseline) × A+B com ≥ 1 componente × A+B com proporção.

Detalhes: [[06-avaliacao]].

---

## 10. Ferramenta

Aplicação web com cadastro manual de requisitos, ADRs, módulos e componentes; registro de substituições; vínculos de rastreabilidade; detecção de sobreposição; cálculo de CA e ID; classificação Recoberto/Órfão; painel e matriz de rastreabilidade.

Detalhes e decisões pendentes (stack, modelo de dados): [[07-ferramenta]].

---

## 11. Resultados esperados

| Dimensão | Contribuição |
|---|---|
| **Conceitual** | Modelo de rastreabilidade `Requisito → ADR → Módulo`, com cobertura estática e impacto dinâmico como métricas distintas e complementares |
| **Prática** | Ferramenta web funcional que sinaliza módulos órfãos após uma nova decisão |
| **Empírica** | Evidências iniciais de aceitação percebida e de compatibilidade da detecção automática com especialistas |

---

## 12. Questões em aberto

Resumo do que ainda precisa ser decidido (o plano completo está no `TODO.md` do repositório):

1. **Hierarquia módulo × componente × contêiner** e como componentes se propagam para módulos → [[02-modelo-conceitual#Questões em aberto]].
2. **Estados da ADR** e a definição exata de "vigente" → [[02-modelo-conceitual#Estados de uma ADR]].
3. **Módulo afetado que não é nem Recoberto nem Órfão** (Condição B com ADR antiga ainda vigente) → [[03-metricas#Questões em aberto]].
4. **Condição B** deve considerar só ADRs anteriores vigentes?
5. **Denominadores diferentes** entre CA (só críticos) e ID (todo o escopo anterior).
6. **Critério de módulo crítico** e **limiar de sobreposição**.
7. **Circularidade** do estudo de acurácia e ausência de relação de dependência entre módulos → [[06-avaliacao#Riscos e questões em aberto]].
8. **Só "afetado" é avaliado**; Recoberto/Órfão ficaria sem validação.
9. **Tamanhos amostrais** e origem dos cenários de teste.
10. **Posicionamento frente a ferramentas existentes** (adr-tools, MADR, Log4brains, Structurizr) → [[08-referencias]].

---

## 13. Mapa das notas

| Nota | Conteúdo |
|---|---|
| [[01-contexto-problema]] | Contexto, problema, pergunta, objetivos, delimitação |
| [[02-modelo-conceitual]] | Entidades, relações, estados da ADR |
| [[03-metricas]] | CA, ID, TO, classificação, questões abertas |
| [[04-framework/index\|04-framework]] | Fundamentação, processo, artefatos |
| [[05-metodologia-dsr]] | Fases da DSR |
| [[06-avaliacao]] | TAM, estudo de detecção, variantes |
| [[07-ferramenta]] | Requisitos e decisões da aplicação |
| [[08-referencias]] | Referências e leituras |

---

## 14. Síntese visual

```
REQUISITOS
    │
    ▼
   ADR
    │
    ├──────────────► MÓDULOS / COMPONENTES
    │
    ▼
NOVA ADR
    │
    ├── supersedes ───────────────┐
    │                             │
    └── sobreposição de escopo ───┤
                                  ▼
                         MÓDULOS AFETADOS
                                  │
                    ┌─────────────┴─────────────┐
                    ▼                           ▼
               RECOBERTO                    ÓRFÃO
          Nova ADR o cobre        Nenhuma ADR vigente o cobre
```

A proposta desloca o uso de ADRs de um mecanismo predominantemente **documental** para um mecanismo de **rastreabilidade e apoio à governança da evolução arquitetural**: observar não só quais decisões existem, mas quais partes do sistema continuam arquiteturalmente justificadas quando essas decisões mudam.

---

## Legenda

- `supersedes`: a ADR atual **substitui** explicitamente uma ou mais ADRs anteriores.
- `superseded-by`: a ADR atual **foi substituída** por uma ADR posterior.
- **Órfão arquitetural:** módulo afetado por uma decisão que deixou de valer e sem ADR vigente que o justifique.
- **Recoberto:** módulo afetado, mas coberto por uma nova ADR vigente.
