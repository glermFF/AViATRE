# ADR-0002: Esquema canônico de features versionado

- Status: Proposto
- Data: 2026-11-12

## Contexto
Modelos treinados em CSV de dataset precisam classificar fluxos extraídos ao vivo. Se as colunas, a ordem ou o extrator diferem, o modelo recebe entradas que não significam o que ele aprendeu (*train/serve skew*). Os nomes de colunas também variam entre versões e datasets (ex.: "Total Backward Packets" × "Tot Bwd Pkts").

## Decisão
- Um `FeatureSpec` versionado define nomes (snake_case) e ordem das features.
- Toda entrada passa por `normalize_column_name` e `vectorize`. Valores não finitos viram NaN; a imputação é do pipeline.
- Todo modelo grava `ModelMetadata` (versão do esquema, nomes das features, semente, hash do dataset). O serviço de detecção só carrega o modelo se `check_compatible` passar.
- A lista atual é rascunho (`1-draft`). Será fechada na Fase 1 a partir da interseção das colunas dos datasets escolhidos e da importância das features.
- O mesmo extrator (versão fixa) alimenta treino e inferência. Qual extrator: pergunta Q5.

## Alternativas consideradas
- Usar todas as ~80 colunas do extrator: mais sinal, porém mais acoplamento a cada dataset/versão e mais risco de vazamento de informação identificadora.
- Sem esquema formal (apenas DataFrames): mais rápido no início, falha de forma silenciosa depois.

## Consequências
Pequeno custo inicial; falhas de incompatibilidade viram erro explícito. Mudar a lista exige nova versão e retreino.

## Revisitar quando
Fechar a lista de features (Fase 1) ou trocar de extrator.
