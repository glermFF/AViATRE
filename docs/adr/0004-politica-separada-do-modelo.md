# ADR-0004: Política de resposta separada do modelo, com modos e invariantes de segurança

- Status: Proposto
- Data: 2026-11-12

## Contexto
Um falso positivo isolado não deve derrubar um host. Isolar é decisão de política (quantos fluxos, em quanto tempo, de qual origem), não saída direta do classificador.

## Decisão
- O serviço `policy` consome `Detection` e produz `MitigationAction`.
- Três modos: `observe` (só registra/alerta), `recommend` (sugere), `enforce` (executa). Todo ciclo começa em `observe`.
- Invariantes no próprio contrato: `observe` não bloqueia; todo bloqueio tem TTL > 0; tipo de ação e tipo de alvo são consistentes.
- **Fail-open** e **allowlist** (controlador, gateway) que nunca é isolada; ficam na política, não no modelo.
- Unidade de isolamento (host ou fluxo): pergunta Q4, decidida no início da Fase 4.

## Alternativas consideradas
- Bloquear direto quando `score` passa do limiar: simples, mas agressivo e difícil de medir "isolamento indevido".

## Consequências
Permite medir separadamente a qualidade do modelo e a qualidade da política (tempo até mitigação, isolamento indevido).

## Revisitar quando
Incluir janelas por host/segmento ou resposta graduada (limitar taxa antes de bloquear).
