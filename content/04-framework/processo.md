---
title: Processo operacional
---

```
┌──────────────────────────────────┐
│ 1. Requisitos e Escopo           │
└─────────────────┬────────────────┘
                  ▼
┌──────────────────────────────────┐
│ 2. Registro e Vinculação de ADRs │
└─────────────────┬────────────────┘
                  ▼
┌──────────────────────────────────┐
│ 3. Análise de Cobertura e Impacto│
└─────────────────┬────────────────┘
                  ▼
┌──────────────────────────────────┐
│ 4. Diagnóstico e Governança      │
└──────────────────────────────────┘
```

## Etapa 1 — Formalização de requisitos e escopo

- Cadastro dos requisitos com implicações arquiteturais.
- Construção do catálogo de módulos e componentes, em estrutura compatível com o C4.
- Marcação dos módulos **críticos** conforme [[03-metricas#Módulo crítico|critério prévio]].

## Etapa 2 — Registro e vinculação das decisões

ADRs são registradas pela [[04-framework/artefatos#Ficha de ADR Expandida|Ficha de ADR Expandida]] e associadas a:

- requisitos que motivaram a decisão;
- módulos e componentes afetados;
- ADRs que ela substitui;
- ADRs que depois a substituírem.

Essas relações formam a **matriz de rastreabilidade**.

## Etapa 3 — Análise de cobertura e impacto

**Análise estática:** verifica quais módulos críticos têm ao menos uma ADR vigente e calcula a Cobertura Arquitetural.

**Análise dinâmica**, ao cadastrar uma nova ADR:

1. identifica ADRs substituídas via `supersedes`;
2. identifica ADRs anteriores com escopo sobreposto;
3. determina os módulos potencialmente afetados;
4. calcula o Impacto de Decisão;
5. verifica o estado arquitetural de cada módulo após a mudança.

## Etapa 4 — Diagnóstico e governança

Módulos afetados são classificados como **Recobertos** ou **Órfãos Arquiteturais** e exibidos em um painel, para apoiar atualização documental, revisão arquitetural ou nova decisão.
