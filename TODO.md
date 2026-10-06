# TODO — Definição do projeto VAL-TRACE

Fora de `content/` de propósito: não é publicado no site. Ordenado por prioridade; os itens do bloco A bloqueiam o modelo de dados.

## A. Fechar o modelo (bloqueia o desenvolvimento)

- [ ] **Hierarquia contêiner → módulo → componente.** Decidir se módulo = contêiner C4 ou se há 3 níveis; cada componente pertence a exatamente um módulo? Como a sobreposição de componentes vira "módulo afetado"? (`02-modelo-conceitual`)
- [ ] **Máquina de estados da ADR.** Estados, transições permitidas e definição de "vigente". Resolver "aceita × vigente" e "substituída × superseded-by". (`02-modelo-conceitual`)
- [ ] **Tratar `supersedes` completo:** cadeias A→B→C, ciclos (proibir?), substituição parcial, geração automática do `superseded-by`.
- [ ] **Fechar a taxonomia dos afetados.** Resolver o caso "afetado pela Condição B com ADR antiga ainda vigente e fora da nova ADR": criar 3ª classe (ex.: *Sobreposição / Conflito potencial*) ou restringir a Condição B. (`03-metricas`)
- [ ] **Condição B só com ADRs vigentes?** Decidir e escrever a regra.
- [ ] **Denominador do ID:** todos os módulos do escopo anterior ou só os críticos? Alinhar com a CA ou justificar a diferença.
- [ ] **Escrever definições formais** (conjuntos e predicados: `Vigente(adr)`, `Escopo(adr)`, `Afetado(m, nova)`, `Recoberto(m)`, `Orfao(m)`) em uma nota única, sem ambiguidade.

## B. Definições operacionais das métricas

- [ ] **Critério de módulo crítico:** escolher atributos (complexidade, dependentes, dependências, centralidade), limiares e justificativa; decidir como o usuário informa os valores sem integração.
- [ ] **Limiar de sobreposição:** além de "≥ 1", definir 1–2 variantes (proporção/Jaccard) para comparar na avaliação.
- [ ] **Exemplo numérico completo** (6–8 módulos, 3–4 ADRs, 1 nova ADR) calculando CA antes/depois, ID, Recoberto/Órfão e TO. Serve de teste de sanidade das fórmulas e depois de caso de teste da ferramenta. (`03-metricas`)
- [ ] **Casos limite:** escopo anterior vazio (divisão por zero), nenhum módulo crítico, ADR sem módulos, módulo sem ADR.

## C. Desenho da avaliação

- [ ] **Origem dos cenários:** projetos open-source com ADRs reais (levantar candidatos), cenários sintéticos, ou ambos. Definir a quantidade (meta inicial: 15–30 cenários).
- [ ] **Cenários que fujam da circularidade:** incluir impactos semânticos/indiretos, e decidir se são limitação declarada ou se o modelo ganha relação de dependência entre módulos.
- [ ] **Especialistas:** quantos (mín. 2–3), perfil (experiência com arquitetura/ADRs), como recrutar, protocolo de rotulação e como resolver divergências.
- [ ] **Concordância entre avaliadores:** escolher a medida (Cohen/Fleiss κ) e o mínimo aceitável antes de usar como ground truth.
- [ ] **Avaliar também Recoberto/Órfão**, não só "afetado".
- [ ] **Desenho das variantes comparadas:** baseline (só Condição A) × A+B(≥1) × A+B(proporção). Hipóteses e como reportar.
- [ ] **TAM:** número de participantes, perfil, tarefa, instrumento (itens em português, escala), análise; decidir se o SUS entra.
- [ ] **Ameaças à validade:** listar (preenchimento manual, viés dos especialistas, tamanho amostral, generalização).
- [ ] **Comitê de ética / TCLE**, se exigido pela instituição para participantes humanos. Confirmar com o orientador.

## D. Fundamentação e posicionamento

- [ ] **Revisão da literatura** com protocolo simples (bases, strings de busca, critérios de inclusão): ADRs, rastreabilidade de decisões, *architectural drift/erosion*, débito técnico arquitetural.
- [ ] **Trabalhos relacionados / ferramentas existentes:** comparar adr-tools, MADR, Log4brains, Structurizr, ArchUnit e afins em uma tabela (o que cobrem × o que o VAL-TRACE adiciona). Deixar a lacuna de pesquisa explícita.
- [ ] **Justificar a tríade C4 + 4+1 + IAF** pelo efeito no modelo, ou reduzir. (`04-framework/fundamentacao`)
- [ ] **Completar `08-referencias`** com citações verificadas (as atuais estão marcadas "verificar").
- [ ] **Preparar as ADRs do próprio VAL-TRACE** (`content/decisoes/`): decisões de modelo e de stack como dogfooding e como cenário de teste.

## E. Ferramenta

- [ ] **Escolher a stack** e registrar como ADR.
- [ ] **Diagrama ER** do modelo de dados (depois do bloco A).
- [ ] **Fluxo "nova ADR":** simular o impacto antes de aceitar? Quando a ADR muda de status e o cálculo é refeito?
- [ ] **Regras de consistência:** ciclos, ADR vigente sem requisito, módulo sem componente, referências inexistentes.
- [ ] **Importar/exportar:** MADR com front-matter, CSV da matriz, grafo de supersedes.
- [ ] **Dados de exemplo** (seed) com o exemplo numérico do bloco B.
- [ ] **Testes automatizados das métricas** a partir dos casos do bloco B.

## F. Documento e processo

- [ ] **Reunião com o orientador** para validar: escopo, critérios de avaliação, necessidade de ética, cronograma.
- [ ] **Cronograma** por fase DSR, com marcos (modelo fechado → protótipo → coleta → escrita).
- [ ] **Glossário** (ADR, vigente, afetado, recoberto, órfão, módulo, componente, contêiner) para fixar a terminologia.
- [ ] **Padronizar pt-BR** ("registro/equipes") e termos em inglês (*drift*, *ground truth*).
- [ ] **Publicar o site:** confirmar o push do repo e ativar Pages com Source = GitHub Actions.

## Ordem sugerida

1. Bloco A + exemplo numérico do B (fecham o modelo e testam as fórmulas).
2. Definições operacionais restantes do B.
3. Reunião com o orientador (F) com o modelo fechado em mãos.
4. Desenho da avaliação (C) e revisão da literatura (D) em paralelo.
5. Ferramenta (E).
