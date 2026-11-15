# AViATRE
**A**mbiente **Vi**rtual para **A**nálise de **T**ráfego de **RE**des.

## Sobre

O AViMTRE é um **esquema canônico de features** versionado liga extração e modelo. O mesmo extrator alimenta o treino (datasets)
e a inferência (tráfego do laboratório), e um modelo só é carregado se o esquema coincide. Isso evita que um
modelo bom em CSV falhe com tráfego ao vivo.

A proposta é construir um laboratório modular utilizando ferramentas de código aberto e amplamente utilizadas em redes e segurança da informação. O projeto parte de uma imagem Docker personalizada para captura e análise de pacotes, que já possui o Wireshark/TShark instalado, e será expandido com componentes especializados para visualização do tráfego e monitoramento de segurança.

O ambiente será desenvolvido inicialmente como um laboratório controlado, permitindo gerar tráfego entre máquinas, capturá-lo, analisá-lo e visualizar informações relevantes. Em etapas posteriores, o projeto poderá incorporar mecanismos de detecção de eventos, armazenamento de logs, automação com Bash/Python e integração com ferramentas de SIEM.

A proposta prioriza uma arquitetura modular. Cada ferramenta deverá desempenhar uma responsabilidade específica, evitando concentrar captura, análise, detecção e visualização em um único container.