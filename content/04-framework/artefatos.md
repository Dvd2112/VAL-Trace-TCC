---
title: Taxonomia de artefatos
---

## Artefatos de entrada

### Especificação de Requisitos
Requisitos funcionais, não funcionais e restrições com impacto arquitetural, que precisam ser ligados às decisões que os implementam.

### Catálogo de Módulos e Componentes
Representação estruturada dos elementos arquiteturais relevantes. Conteúdo mínimo:

- identificador e nome;
- descrição e tipo;
- contêiner ao qual pertence;
- componentes associados;
- indicador de criticidade;
- atributos usados para determinar a criticidade.

## Artefatos de modelagem

### Ficha de ADR Expandida

Extensão do formato tradicional (MADR / adr-tools):

```yaml
id:
title:
context:
decision:
rationale:
consequences:
status:
architecture-domain:
requirements:
modules:
components:
supersedes:
superseded-by:
created-at:
status-changed-at:
```

Estados: proposta, aceita, vigente, rejeitada, depreciada, substituída. Ver [[02-modelo-conceitual#Estados de uma ADR|questão em aberto]].

## Artefatos de saída

### Matriz de Rastreabilidade Requisito–ADR–Módulo

```
Requisito ↔ ADR ↔ Módulo
```

Permite saber: quais decisões atendem um requisito; quais módulos uma decisão justifica; quais requisitos originaram uma decisão; quais módulos perderam cobertura após a substituição de uma ADR.

### Painel de Cobertura e Impacto

Apresenta:

- percentual de Cobertura Arquitetural;
- percentual de Impacto de Decisão;
- ADRs vigentes e substituídas, com as relações de substituição;
- módulos afetados, recobertos e órfãos;
- relação requisito–ADR–módulo.
