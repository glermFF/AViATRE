### Arquitetura (piloto)
```
                         ┌──────────────────────┐
                         │      REDE / LAN      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                     ┌──────────────────────────┐
                     │       FIREWALL           │
                     │    OPNsense / pfSense    │
                     └────────────┬─────────────┘
                                  │
                                  ▼
                 ┌────────────────────────────────┐
                 │       Docker Network            │
                 │                                │
                 │  ┌──────────────────────────┐  │
                 │  │  Container: Sniffer      │  │
                 │  │                          │  │
                 │  │  TShark / tcpdump        │  │
                 │  │  Bash / Python           │  │
                 │  │  PCAP                    │  │
                 │  └────────────┬─────────────┘  │
                 │               │                │
                 │               ▼                │
                 │  ┌──────────────────────────┐  │
                 │  │       ntopng             │  │
                 │  │  Dashboard de tráfego     │  │
                 │  └──────────────────────────┘  │
                 │                                │
                 │  ┌──────────────────────────┐  │
                 │  │    Suricata              │  │
                 │  │ IDS / IPS / alertas      │  │
                 │  └──────────────────────────┘  │
                 └────────────────────────────────┘
```

**Camada 1** - Rede
Camada representando a rede conectada ao computador operador da aplicação. Todo o tráfego que passa por esse computador consegue analisar parte dos pacotes que transitam por ele, podendo gerar uma estimativa da quantidade de pacotes comuns ou anômalos enviados no fluxo de dados.

**Camada 2** - Captura
Camada do dispositivo de borda instalado em máquina virtual. Por meio dele podem ser gerados regras de firewall e monitorar tráfego que passa pela interface do computador. O ONPsense oferece um dashboard apresentando os pacotes que interagiram com a máquina, a lista e definição de regras, requisições e dispositivos conectados.

**Camada 3** - Visualização/Análise
Camada ambientada na rede docker ambientada em containers com pipeline de análise do tráfego coletado. Dentro existe a grande combinação de arquivos de execução python, bash junto as ferramentas de sniffer de rede Tshark e TcpDump. Os scripts possuem a execução de subrotinas  que coletam os pacotes de rede, gerando um arquivo PCAP que é enviado ao **ntopng**, um dashboard web que apresenta mais detalhes sobre os pacotes de maneira organizada. Com os dados em mãos entra o **Suricata**, ferramenta com recursos de IDS para envio de alertas em caso de detecção de anomalias.
### Firewall e topologia

O firewall não será inicialmente tratado como uma aplicação dentro do mesmo container do analisador.

A proposta é utilizar uma máquina virtual dedicada, como **OPNsense**, para representar o firewall e roteador do laboratório. Essa abordagem permite separar as funções de infraestrutura de rede das funções de análise executadas nos containers.

Uma topologia inicial poderá ser representada como:

```text
                         INTERNET
                            │
                            ▼
                    ┌───────────────┐
                    │    OPNsense   │
                    │    Firewall   │
                    └───────┬───────┘
                            │
                         REDE/LAN
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
          Cliente         Servidor       Sensor
                                           │
                                           ▼
                                   ┌──────────────┐
                                   │ Docker Host  │
                                   │              │
                                   │ TShark       │
                                   │ Suricata     │
                                   │ ntopng       │
                                   └──────────────┘
```

A forma de disponibilizar o tráfego ao sensor deverá ser definida durante a implementação. Dependendo do cenário, poderão ser utilizados recursos como interface em modo host, bridge, port mirroring/SPAN ou uma interface dedicada para captura.