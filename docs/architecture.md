# Arquitetura do AViATRE

> Status: Fase 0. Decisões detalhadas em `docs/adr/`.

## Visão geral

```
                 ┌────────────── OFFLINE (experimentos) ──────────────┐
  Datasets ───►  │ ingest ─► canonical features ─► train/eval ─► model│
  (CIC-IDS2017,  │ (Parquet)   (schema v1)         (RF/DT/CatBoost)   │
   InSDN, ...)   │                     ▲                  │ registry  │
                 └─────────────────────┼──────────────────┼───────────┘
                                       │ mesmo schema     ▼
  ┌──── LAB (Mininet + Ryu) ────┐   ┌──┴────────┐   ┌───────────┐
  │ topologia + geradores       │──►│ capture   │──►│ extractor │
  │ benigno / ataque (scenarios)│   │ (tshark)  │   │ (flows)   │
  └──────────▲──────────────────┘   └───────────┘   └─────┬─────┘
             │                                            ▼ FlowRecord
     ┌───────┴───────┐   Detection    ┌───────────┐   ┌──────────┐
     │ actuator (Ryu)│◄── Action ─────│  policy   │◄──│ detector │
     │ regra de drop │                │  engine   │   │ (API)    │
     └───────────────┘                └─────┬─────┘   └──────────┘
                                            ▼
                                  storage + métricas + dashboard
```

## Princípio central

**Um contrato entre extração e modelo.** O esquema canônico de features (`core/aviatre_core/features.py`)
é versionado e compartilhado. Datasets e tráfego ao vivo passam pela mesma normalização e pela mesma ordem
de colunas. Um modelo só é carregado se o esquema dele coincide com o esquema em uso (`check_compatible`).
Isso evita o *train/serve skew*: modelo treinado em CSV que falha com tráfego ao vivo.

## Componentes

| Componente | Responsabilidade | Entrada → Saída | Fase |
|---|---|---|---|
| `core/` | Contratos: esquema de features, mensagens, interface `Detector` | — | 0 |
| `experiments/` | Ingestão, treino, avaliação, tabelas de resultados | datasets → modelos + métricas | 1 |
| `lab/` | Topologias Mininet, cenários e geradores de tráfego | cenário YAML → tráfego | 2 |
| `services/capture` | Captura de pacotes (evolução do atual `services/sniffer`) | tráfego → pcap | 2 |
| `services/extractor` | Pcap → fluxos no esquema canônico | pcap → `FlowRecord` | 2 |
| `services/detector` | Inferência com o modelo selecionado | `FlowRecord` → `Detection` | 3 |
| `services/policy` | Agrega detecções e decide a ação | `Detection` → `MitigationAction` | 4 |
| `services/actuator` | Aplica regras no plano de dados (Ryu) | `MitigationAction` → regra OpenFlow | 4 |

## Contratos (em `core/aviatre_core/schemas.py`)

- `FlowRecord`: `flow_id, ts, src_ip, dst_ip, src_port, dst_port, protocol, features, schema_version, label?`
- `Detection`: `flow_id, ts, score (0..1), is_attack, label, model_id, schema_version, latency_ms?`
- `MitigationAction`: `action_id, ts, mode, type, target_kind, target, ttl_s, reason, detection_flow_ids`

Invariantes de segurança codificadas e testadas:
modo `observe` nunca bloqueia; todo bloqueio tem TTL > 0; `isolate_host` exige alvo host e `drop_flow` exige alvo fluxo.

## Fluxos

- **Offline:** dataset → `canonicalize` + `vectorize` → `Detector.fit` → `ModelMetadata` salvo junto ao modelo → métricas.
- **Online:** tráfego do lab → extrator → `FlowRecord` → `Detector.predict_proba` → `Detection` → política → ação.

## Modos de operação da política

`observe` (só registra e alerta) → `recommend` (sugere, não executa) → `enforce` (executa).
Todo ciclo novo começa em `observe`.

## O que revisitar se o sistema crescer

Fila e armazenamento (Redis/Postgres → Kafka/TimescaleDB), registro de modelos (MLflow), detecção por janela e por host,
segundo estágio multiclasse, pydantic nos contratos se a API pedir.
