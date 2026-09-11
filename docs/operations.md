## Levantamento de Requisitos

### Requisitos funcionais

- **RF01 — Captura de tráfego**
O sistema deverá permitir a captura de tráfego de uma interface de rede disponível ao sensor.

- **RF02 — Armazenamento de capturas**
O sistema deverá permitir salvar o tráfego capturado em arquivos PCAP/PCAPNG.

- **RF03 — Filtragem de tráfego**
O sistema deverá permitir aplicar filtros de captura e análise utilizando recursos do TShark/Wireshark.

- **RF04 — Análise de pacotes**
O sistema deverá permitir analisar informações dos pacotes, incluindo endereços IP, portas, protocolos e outros campos relevantes.

- **RF05 — Detecção de eventos**
O ambiente deverá permitir a utilização do Suricata para identificar eventos e padrões de tráfego potencialmente suspeitos.

- **RF06 — Visualização**
O ambiente deverá disponibilizar um dashboard Web para visualização das informações de tráfego.

- **RF07 — Automação**
O projeto deverá permitir a execução de tarefas automatizadas através de scripts Bash e, posteriormente, Python.

- **RF08 — Gerenciamento de serviços**
Os principais componentes containerizados deverão poder ser iniciados e gerenciados através do Docker Compose.

- **RF09 — Separação de responsabilidades**
Captura, análise, detecção e visualização deverão ser componentes independentes sempre que tecnicamente viável.

- **RF10 — Ambiente controlado**
O projeto deverá permitir a reprodução de cenários de tráfego em um laboratório controlado, evitando depender inicialmente de uma rede de produção.

### Requisitos não funcionais

- **RNF01 — Containerização**
Os componentes do ambiente deverão utilizar containers sempre que a ferramenta permitir.

- **RNF02 — Modularidade**
A arquitetura deverá permitir adicionar, remover ou substituir componentes sem reconstruir todo o ambiente.

- **RNF03 — Reprodutibilidade**
A configuração do ambiente deverá ser documentada e automatizada para facilitar sua reprodução em outra máquina.

- **RNF04 — Código aberto**
Deverão ser priorizadas ferramentas abertas e amplamente utilizadas na área de redes e segurança.

- **RNF05 — Persistência**
Capturas e logs importantes deverão ser armazenados em volumes persistentes, evitando perda de dados quando containers forem recriados.

- **RNF06 — Segurança**
O ambiente deverá ser executado preferencialmente em uma rede de laboratório isolada ou controlada.

- **RNF07 — Extensibilidade**
A arquitetura deverá permitir futuras integrações com ferramentas de SIEM, armazenamento de eventos e aplicações desenvolvidas pelo próprio projeto.

---
### Escopo Planejado
O desenvolvimento poderá ocorrer em etapas:

```text
Fase 1
Docker + TShark
        ↓
Captura de tráfego
        ↓
Arquivos PCAP

Fase 2
TShark + scripts Bash/Python
        ↓
Extração e processamento
        ↓
Dados estruturados

Fase 3
Suricata
        ↓
Detecção de eventos
        ↓
Logs e alertas

Fase 4
ntopng
        ↓
Dashboard
        ↓
Monitoramento visual
```

O objetivo inicial não é reproduzir integralmente plataformas como Security Onion ou Malcolm, mas utilizar suas arquiteturas como referência e construir progressivamente um ambiente próprio, modular e compreensível.

A evolução para componentes mais sofisticados deverá ocorrer somente depois que as funções básicas de captura, análise, detecção e visualização estiverem funcionando de forma independente.