# ADR-0001: Pacote Python compartilhado + containers por ferramenta de terceiros

- Status: Proposto
- Data: 2026-11-12

## Contexto
O README original pede "uma responsabilidade por ferramenta, sem concentrar captura, análise, detecção e visualização em um único container". O risco oposto é criar um serviço por função de código, com custo de rede, versionamento e depuração sem benefício para um projeto de uma pessoa.

## Decisão
- **Containers** separam ferramentas de terceiros com ciclo de vida próprio: Mininet, Ryu, tshark/capture, extrator.
- **Lógica de domínio** (esquema, contratos, interface `Detector`, avaliação) vive em um pacote Python único, `aviatre-core`, instalado em todas as imagens que precisam dele.
- Os serviços próprios (`detector`, `policy`) são finos: carregam o `core` e expõem uma interface.
- Contratos como dataclasses imutáveis com validação (somente biblioteca padrão). A API da Fase 3 pode usar pydantic na borda.

## Alternativas consideradas
- Um microsserviço por módulo lógico: mais isolamento, porém duplica contratos e multiplica a configuração.
- Um único container "faz-tudo": simples, mas contradiz o princípio do README e dificulta trocar ferramentas.

## Consequências
Menos peças móveis; contratos testados uma vez. Custo: as imagens dependem da versão do `core`, então ela precisa ser fixada nas imagens.

## Revisitar quando
Um módulo precisar escalar ou ser implantado de forma independente.
