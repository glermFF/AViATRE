# ADR-0003: Interface `Detector` única, binária, com aprendizado online opcional

- Status: Proposto
- Data: 2026-11-12

## Contexto
O TCC compara vários algoritmos (RF, DT, CatBoost e, no futuro, abordagens online como Isolation Forest online) e precisa rodá-los tanto offline quanto no pipeline ao vivo.

## Decisão
- `Detector` define `fit`, `predict_proba` (P(ataque) em [0,1], shape `(n,)`), `predict(threshold)` e `partial_fit` opcional (`supports_online`).
- A classe base usa *template method*: as checagens de formato e de intervalo ficam na base, e as subclasses implementam `_fit` e `_predict_proba`.
- O detector é **binário**. O tipo de ataque (multiclasse) fica para um segundo estágio, a decidir na Fase 1 (Q3).
- Modelos são salvos junto a um `ModelMetadata`.

## Alternativas consideradas
- Interface do scikit-learn diretamente: familiar, mas não carrega a guarda de esquema nem a noção de modo online.
- Multiclasse desde já: mais informativo para sugerir contramedidas (como no MARIA), mas amarra o contrato a um conjunto fixo de classes antes de escolher os datasets.

## Consequências
Trocar o algoritmo não muda o restante do sistema. Custo: um adaptador fino por biblioteca.

## Revisitar quando
Decidir pelo segundo estágio multiclasse ou incluir detectores sequenciais/por janela.
