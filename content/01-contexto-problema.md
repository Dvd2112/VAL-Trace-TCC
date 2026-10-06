---
title: Contexto, problema e objetivos
---

## Contextualização

Sistemas de software evoluem, e decisões arquiteturais são revisadas, substituídas ou complementadas ao longo do tempo. Em muitos projetos há uma **desconexão** entre:

- os requisitos que motivaram uma solução;
- as decisões arquiteturais registradas;
- os módulos efetivamente afetados por essas decisões.

Essa falta de rastreabilidade contribui para o **desvio arquitetural** (*architectural drift*), em que a implementação diverge da arquitetura planejada, e para o acúmulo de **débito técnico**.

## O papel e o limite das ADRs

*Architecture Decision Records* (ADRs) registram decisões com contexto, alternativas e justificativa. Porém, **registrar não é rastrear**: uma ADR sozinha não garante a ligação entre requisitos, decisões e componentes.

Quando uma decisão é substituída, os campos `supersedes` e `superseded-by` indicam a relação entre ADRs, mas os **efeitos sobre os módulos** antes abrangidos raramente são avaliados de forma sistemática.

## Órfãos arquiteturais

Módulos podem continuar associados, direta ou indiretamente, a decisões que deixaram de ser válidas, sem que uma decisão vigente os justifique. Neste trabalho, são os **órfãos arquiteturais**.

> [!note] Legenda
> - `supersedes`: a ADR atual **substitui** explicitamente uma ou mais ADRs anteriores.
> - `superseded-by`: a ADR atual **foi substituída** por uma ADR posterior.

## Pergunta de pesquisa

> Como um framework e uma ferramenta de rastreabilidade, combinando relações explícitas entre ADRs e detecção automática de sobreposição de escopo, podem quantificar a cobertura arquitetural de um sistema e o impacto de novas decisões arquiteturais sobre módulos previamente existentes?

## Objetivos

### Geral

Propor o framework **VAL-TRACE** e desenvolver uma ferramenta web que calcule **Cobertura Arquitetural** e **Impacto de Decisão**, usando relações explícitas entre ADRs e análise automática de sobreposição dos módulos/componentes declarados.

### Específicos

- Definir o modelo de dados (requisitos, ADRs, módulos, relações de rastreabilidade).
- Definir a estrutura estendida de ADR (com `supersedes` e `superseded-by`).
- Definir a vinculação **requisito ↔ ADR ↔ módulo/componente**.
- Desenvolver a aplicação web de cadastro e gestão manual.
- Implementar o cálculo de [[03-metricas#Cobertura Arquitetural|Cobertura Arquitetural]] e de [[03-metricas#Impacto de Decisão|Impacto de Decisão]].
- Implementar a detecção automática de sobreposição de escopo entre ADRs.
- Classificar módulos afetados como **Recobertos** ou **Órfãos**.
- Avaliar a aceitação percebida (TAM).
- Avaliar a detecção de impacto comparando resultado automático com especialistas.

## Delimitação

Nesta versão o VAL-TRACE é uma ferramenta **independente**, sem integração com Git, análise estática ou plataformas externas. Requisitos, ADRs, módulos e componentes são cadastrados **manualmente**.

A delimitação concentra a investigação na validade do modelo conceitual, das regras de rastreabilidade e das métricas, evitando que a integração tecnológica introduza variáveis adicionais. Integrações ficam como evolução futura.

## Fora de escopo (por ora)

- Eliminar comprovadamente o *architectural drift* ou o débito técnico. O estudo investiga se a rastreabilidade ajuda a **identificar antecipadamente** situações que os favorecem.
- Extração automática de métricas do código-fonte.
- Integração com repositórios e CI.
