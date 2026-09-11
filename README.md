# AViATRE
AViATRE - Ambiente Virtual para Análise de Tráfego de Redes

## Sobre

O AViMTRE é um ambiente virtualizado e containerizado destinado ao monitoramento, captura, análise e visualização de tráfego de redes.

A proposta é construir um laboratório modular utilizando ferramentas de código aberto e amplamente utilizadas em redes e segurança da informação. O projeto parte de uma imagem Docker personalizada para captura e análise de pacotes, que já possui o Wireshark/TShark instalado, e será expandido com componentes especializados para visualização do tráfego e monitoramento de segurança.

O ambiente será desenvolvido inicialmente como um laboratório controlado, permitindo gerar tráfego entre máquinas, capturá-lo, analisá-lo e visualizar informações relevantes. Em etapas posteriores, o projeto poderá incorporar mecanismos de detecção de eventos, armazenamento de logs, automação com Bash/Python e integração com ferramentas de SIEM.

A proposta prioriza uma arquitetura modular. Cada ferramenta deverá desempenhar uma responsabilidade específica, evitando concentrar captura, análise, detecção e visualização em um único container.