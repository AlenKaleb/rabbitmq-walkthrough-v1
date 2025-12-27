# Exportação anotada do código

Esta pasta contém a **exportação do código da branch atual** em arquivos `.md` com anotações técnicas.

## Visão geral de arquitetura

- **Dois projetos principais:**
  - `RabbitMQWalkthrough.Core`: biblioteca com infraestrutura de mensageria, métricas e acesso a dados.
  - `WebApplicationEntrypoint`: aplicação ASP.NET Core que expõe APIs, inicia workers e configura a infraestrutura.
- **Infraestrutura externa (Docker Compose):**
  - RabbitMQ (com Management API) para mensageria.
  - Postgres para persistência de mensagens e métricas.
  - Grafana/pgAdmin para observabilidade/administracao.
- **Padrões e boas práticas presentes:**
  - **Injeção de dependência (DI)** para construir conexões, serviços e workers.
  - **Extension Methods** para encapsular utilitários (serialização, controle de QoS, temporização).
  - **BackgroundService** para execução contínua de coleta de métricas.
  - **Separação de responsabilidades**: camadas de infraestrutura (mensageria, dados, métricas) isoladas do entrypoint web.
  - **Resiliência** com Polly para retry na criação de conexões.

## Como ler os arquivos

Cada arquivo `.md` nesta exportação contém:

1. **Propósito** do arquivo na arquitetura.
2. **Trechos de código** (original) em blocos.
3. **Anotações técnicas** explicando decisões, padrões e boas práticas.

> Observação: arquivos gerados automaticamente (ex.: `package-lock.json`) são documentados em alto nível para explicar a estrutura e o propósito sem detalhar dependências individuais.
