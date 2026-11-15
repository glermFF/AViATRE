# Roadmap

Cada fase entrega algo utilizável e só depende das anteriores.

| Fase | Entrega | Pronto quando |
|---|---|---|
| 0. Fundação | Requisitos, ADRs, esquema canônico, estrutura do repositório, compose mínimo | `make test` passa e `docker compose --profile offline up` sobe com healthcheck verde |
| 1. Núcleo offline | Ingestão de 1–2 datasets, runner por configuração (RF, DT, CatBoost), protocolos baseline e de generalização, repetições com IC | Uma tabela reproduzível modelos × datasets × protocolo sai de um único comando |
| 2. Laboratório | Topologia Mininet, geradores benigno/ataque, repetição de PCAP de dataset pelo extrator, checagem de compatibilidade de distribuição | O modelo treinado classifica fluxos do lab com métrica medida |
| **Decisão** | **Ferramenta ativa × estudo de modelos** | Com as Fases 0–2 já existe um TCC sólido; decidir com dados |
| 3. Detecção online | Detector como API, armazenamento, dashboard, latência medida | Um ataque no lab aparece no dashboard com tempo de detecção |
| 4. Resposta | Policy engine e actuator Ryu (dry-run → enforce) | Host atacante isolado com tráfego legítimo preservado; tempo até mitigação e isolamento indevido medidos |
| 5. Extensões | Drift, aprendizado online seletivo, ID de segmento como feature, infraestrutura real | Trabalho futuro |

