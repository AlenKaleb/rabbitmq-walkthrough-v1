# launchSettings.json

## Propósito
Configura o perfil de execução do Docker Compose via Visual Studio.

## Código anotado

```json
{
  "profiles": {
    "Docker Compose": {
      "commandName": "DockerCompose",
      "commandVersion": "1.0",
      "composeLaunchAction": "LaunchBrowser",
      "composeLaunchServiceName": "webapplicationentrypoint",
      "composeLaunchUrl": "{Scheme}://localhost:{ServicePort}",
      "serviceActions": {
        "grafana": "StartWithoutDebugging",
        "rabbitmq": "StartWithoutDebugging",
        "pgadmin4": "StartWithoutDebugging",
        "webapplicationentrypoint": "StartDebugging",
        "postgres": "StartWithoutDebugging"
      }
    }
  }
}
```

### Anotações técnicas
- **Iniciação orquestrada**: define quais serviços serão iniciados com/sem debugging.
- **LaunchUrl**: abre automaticamente o serviço web principal.
