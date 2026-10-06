---
title: Ferramenta web
---

Aplicação web **independente** do VAL-TRACE, com cadastro manual. Esta nota reúne os requisitos e decisões de projeto da ferramenta; ela será expandida quando o modelo de dados estiver fechado.

## Funcionalidades previstas

- Cadastro de requisitos, ADRs, módulos e componentes.
- Registro de relações de substituição entre ADRs.
- Vínculos entre requisitos, ADRs e módulos.
- Detecção automática de sobreposição de escopo.
- Cálculo da Cobertura Arquitetural e do Impacto de Decisão.
- Identificação de módulos recobertos e possíveis órfãos.
- Relatórios de rastreabilidade e governança (painel e matriz).

## Fora desta versão

- Integração com Git, CI e análise estática.
- Importação automática de métricas do código.

## A definir

- Stack tecnológica (front-end, back-end, persistência).
- Modelo de dados detalhado (diagrama entidade-relacionamento).
- Fluxo de cadastro de uma nova ADR e momento do cálculo do impacto ("simular antes de aceitar"?).
- Validações de consistência (ciclos de `supersedes`, ADRs vigentes sem requisito, módulos sem componente).
- Importar/exportar: arquivos MADR (`.md` com front-matter), CSV da matriz, grafo de substituições.

Ver [[04-framework/artefatos]] e [[03-metricas]].
