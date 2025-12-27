# WebApplicationEntrypoint/Workers/MetricsWorker.cs

## Propósito
Worker em background que coleta métricas periodicamente.

## Código anotado

```csharp
public class MetricsWorker : BackgroundService
{
    private readonly ILogger<MetricsWorker> _logger;
    private readonly MetricsService metricsService;

    public MetricsWorker(ILogger<MetricsWorker> logger, MetricsService metricsService)
    {
        this._logger = logger;
        this.metricsService = metricsService;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            this._logger.LogInformation("MetricsWorker running at: {time}", DateTimeOffset.Now);
            try
            {
                this.metricsService.CollectAndStoreAsync();
                await Task.Delay(1000, stoppingToken);
            }
            catch (OperationCanceledException)
            {
                break;
            }
        }
    }
}
```

### Anotações técnicas
- **`BackgroundService`**: padrão do ASP.NET Core para tarefas contínuas.
- **`Task.Delay` com `CancellationToken`**: permite parada graciosa.
- **Observação**: `CollectAndStoreAsync()` é chamado sem `await`; se necessário, `await` pode garantir tratamento de erro assíncrono.
