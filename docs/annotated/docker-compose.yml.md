# docker-compose.yml

## Propósito
Define o serviço principal da aplicação web e a rede Docker.

## Código anotado

```yaml
version: '3.4'

services:
  webapplicationentrypoint:
    image: ${DOCKER_REGISTRY-}webapplicationentrypoint
    build:
      context: .
      dockerfile: WebApplicationEntrypoint/Dockerfile
    environment: {
            "TZ": "America/Sao_Paulo"
    }
    networks:
    - walkthrough-net

networks:
  walkthrough-net:
    driver: bridge
```

### Anotações técnicas
- **Imagem customizada**: construída a partir do Dockerfile do projeto web.
- **`TZ`**: garante timezone consistente entre serviços.
- **Rede dedicada**: permite comunicação entre containers por hostname.
