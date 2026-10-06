---
title: Modelo conceitual
---

O VAL-TRACE estrutura a rastreabilidade em três elementos:

```
Requisito → Decisão Arquitetural (ADR) → Módulo/Componente
```

## Entidades

| Entidade | Descrição |
|---|---|
| **Requisito** | Necessidade funcional, não funcional ou restrição que influencia a arquitetura. |
| **ADR** | Decisão técnica tomada para atender a um conjunto de requisitos. Ver [[04-framework/artefatos#Ficha de ADR Expandida|Ficha de ADR]]. |
| **Módulo** | Unidade estrutural do sistema afetada por decisões. |
| **Componente** | Parte de um módulo/contêiner; é o grão usado na detecção de sobreposição. |

## Relações

Uma ADR pode:

- atender a **um ou mais requisitos**;
- abranger **um ou mais módulos/componentes**;
- **substituir** uma ADR anterior (`supersedes`);
- ser **substituída** por uma posterior (`superseded-by`).

A partir disso o framework identifica o **estado atual da cobertura** e os **efeitos da evolução das decisões**.

## Estados de uma ADR

Estados possíveis: *proposta*, *aceita*, *vigente*, *rejeitada*, *depreciada*, *substituída*.

> [!question] Questão em aberto
> "Aceita" e "vigente" se sobrepõem, e "substituída" coincide com ter `superseded-by`. É preciso definir a **máquina de estados** e o que exatamente conta como **vigente**, pois isso alimenta tanto a Cobertura quanto a classificação de Órfão. Ver [[#Questões em aberto]].

## Questões em aberto

> [!question] Hierarquia módulo × componente × contêiner
> A ADR possui os campos `modules` e `components`. A sobreposição é calculada por **componente**, mas "afetado" é definido por **módulo**. É necessário definir formalmente a hierarquia (um componente pertence a exatamente um módulo? módulo equivale a contêiner C4?) e como os componentes se propagam para módulos.

> [!question] Relação de dependência entre módulos
> Hoje, dependentes e dependências são só atributos numéricos de [[03-metricas#Módulo crítico|criticidade]], não arestas do modelo. Decidir se impactos **indiretos** (via dependência) são limitação declarada ou entram no modelo.

> [!question] Substituição parcial e cadeias
> Tratar `A → B → C`, ciclos, substituição que cobre só parte do escopo e a consistência entre `supersedes` e `superseded-by` (gerar o segundo automaticamente a partir do primeiro?).
