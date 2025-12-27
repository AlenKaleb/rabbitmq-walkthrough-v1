# infra/rabbitmq/Dockerfile

## Propósito
Constrói uma imagem RabbitMQ com plugins e configurações customizadas.

## Código anotado

```dockerfile
FROM rabbitmq:4-management-alpine

RUN apk add curl

RUN rabbitmq-plugins enable --offline \
    rabbitmq_shovel rabbitmq_shovel_management 

ADD ./rabbitmq.conf /etc/rabbitmq/conf.d/11-custom.conf
```

### Anotações técnicas
- **Base `management`**: habilita a UI/HTTP API.
- **`curl`**: utilizado no healthcheck do compose.
- **Plugins `shovel`**: permitem replicação/encaminhamento de mensagens.
- **Configuração customizada**: adiciona `rabbitmq.conf` com credenciais e métricas.
