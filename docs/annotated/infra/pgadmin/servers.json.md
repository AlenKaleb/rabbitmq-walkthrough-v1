# infra/pgadmin/servers.json

## Propósito
Configuração de servidores do pgAdmin para apontar para o Postgres do compose.

## Código anotado

```json
{
    "Servers": {
        "1": {
            "Name": "RabbitMQ-Walkthrough",
            "Group": "gago-io",
            "Host": "postgres",
            "Port": 5432,
            "Username": "WalkthroughUser",
            "SSLMode": "prefer",
            "MaintenanceDB": "postgres"
        }
    }
}
```

### Anotações técnicas
- **Host interno**: usa o nome do serviço `postgres` na rede do Docker.
- **`SSLMode: prefer`**: tenta SSL quando disponível.
