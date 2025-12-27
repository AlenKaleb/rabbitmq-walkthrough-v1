# WebApplicationEntrypoint/appsettings.json

## Propósito
Configurações padrão de ambiente para logging e hosts permitidos.

## Código anotado

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

### Anotações técnicas
- **Níveis de log**: reduz ruído de logs do framework, mantendo informações relevantes.
- **`AllowedHosts: "*"`**: permite qualquer host (útil para desenvolvimento). Em produção, recomenda-se restringir.
