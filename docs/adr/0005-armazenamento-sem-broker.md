# ADR-0005: Armazenamento simples (Parquet + SQLite) e sem broker de mensagens

- Status: Proposto
- Data: 2026-11-12

## Contexto
O CIC-IDS2017 tem cerca de 2,8 milhões de fluxos. No Mininet, com enlaces de dezenas de Mb/s, a taxa de fluxos é de dezenas a centenas por segundo. A carga cabe em um processo Python em máquina pessoal.

## Decisão
- Offline: arquivos Parquet em `data/` (fora do Git).
- Online (Fases 3–4): SQLite para detecções/ações; se for preciso desacoplar produtor e consumidor, Redis Streams.
- Sem Kafka, sem Postgres por ora.

## Alternativas consideradas
- Kafka/TimescaleDB: adequados para escala que o projeto não tem; custo alto de configuração e de memória.

## Consequências
Menos infraestrutura para manter e reproduzir. Limite: não é um desenho para milhares de fluxos por segundo.

## Revisitar quando
Medições da Fase 3 mostrarem gargalo, ou quando houver mais de um consumidor independente dos eventos.
