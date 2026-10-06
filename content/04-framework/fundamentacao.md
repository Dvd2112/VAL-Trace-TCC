---
title: Fundamentação conceitual
---

## C4 Model

Estrutura o **escopo técnico** associado às ADRs. O foco são dois níveis:

- **Contêiner**: grandes unidades executáveis ou implantáveis.
- **Componente**: delimitação mais fina do escopo afetado por uma decisão.

Permite associar uma ADR a elementos arquiteturais concretos e **comparar seus escopos**, base da detecção de sobreposição.

## Modelo de visões 4+1 (Kruchten)

Referência complementar para organizar perspectivas arquiteturais:

- As visões de **desenvolvimento, processo e física** ajudam a classificar o *tipo* de impacto de uma ADR.
- Os **cenários** funcionam como elementos de validação da arquitetura.

O 4+1 **não entra no cálculo** das métricas; serve para estruturar e interpretar os artefatos.

## Integrated Architecture Framework (IAF)

Referência para **organização e governança** dos artefatos. Uma ADR pode ser classificada por domínio predominante:

- aplicação;
- informação/dados;
- infraestrutura;
- integração.

A classificação facilita interpretar os impactos mostrados pela ferramenta.

## Resumo do papel de cada referência

| Referência | O que alimenta no VAL-TRACE | Entra no cálculo? |
|---|---|---|
| C4 Model | Hierarquia contêiner/componente; grão da sobreposição | Sim (indiretamente) |
| 4+1 | Tipo de impacto; cenários de validação | Não |
| IAF | Domínio da ADR; organização dos artefatos | Não |

> [!question] Questão em aberto
> Verificar se a fundamentação em três referências é justificável pelo que a ferramenta de fato usa. Se 4+1 e IAF só rotulam, considerar reduzir o texto ao que tem efeito no modelo de dados (campo `architecture-domain`, por exemplo).
