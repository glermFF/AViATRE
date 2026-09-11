## Ferramentas

### Docker

Será utilizado como base para a execução dos componentes do ambiente em containers.

### Docker Compose

Será utilizado para orquestrar os serviços do AViMTRE, definir redes, volumes, configurações e dependências entre os containers.

### TShark / Wireshark

O TShark será a ferramenta principal de captura e análise de pacotes na primeira versão do projeto.

A utilização da interface de linha de comando permite integrar a análise com scripts Bash e aplicações Python.

### tcpdump

Poderá ser utilizado como ferramenta complementar de captura e diagnóstico, especialmente em situações nas quais uma ferramenta de captura mais simples seja suficiente.

### Suricata

Será utilizado como mecanismo de IDS/IPS e análise de eventos de segurança.

A integração com o projeto permitirá comparar a análise individual dos pacotes com a detecção baseada em regras e eventos.

### ntopng

Será utilizado inicialmente como dashboard para monitoramento e visualização do tráfego.

A ferramenta permitirá apresentar informações agregadas de hosts, protocolos, aplicações, fluxos e utilização da rede.

### Python

Será incorporado progressivamente para automatizar tarefas e realizar análises que ultrapassem os recursos disponíveis diretamente no TShark.

Possíveis utilizações incluem:

- processamento de dados;
- análise estatística;
- geração de relatórios;
- automação;
- processamento de JSON/CSV;
- criação de regras e análises próprias;
- integração entre componentes.

### Bash/Shell

Será utilizado para tarefas operacionais e automação do ambiente, como:

- inicialização da captura;
- aplicação de filtros;
- gerenciamento de arquivos PCAP;
- rotação e limpeza de capturas;
- execução de rotinas periódicas;
- verificação de serviços.

### OPNsense

Será utilizado como firewall/roteador do laboratório, preferencialmente em uma máquina virtual separada dos containers de análise.

A utilização de um firewall dedicado permite construir uma topologia mais próxima de um ambiente real e controlar o tráfego que será observado pelo sensor.