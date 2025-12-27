# RabbitMQWalkthrough.Core/Infrastructure/Metrics/Collectors/PublisherMetricCollector.cs

## Propósito
Coleta métricas internas de publishers (quantidade e workload configurado).

## Código anotado

```csharp
public class PublisherMetricCollector : IMetricCollector
{
    private readonly PublisherManager publisherManager;

    public PublisherMetricCollector(PublisherManager publisherManager)
    {
        this.publisherManager = publisherManager;
    }

    public Task CollectAndSetAsync(Metric metric)
    {
        metric.WorkerCount = this.publisherManager.Publishers.Count();
        metric.WorkLoadSize = this.publisherManager.Publishers.Sum(it => it.MessagesPerSecond);

        return Task.CompletedTask;
    }
}
```

### Anotações técnicas
- **`WorkerCount`**: quantos publishers ativos estão executando.
- **`WorkLoadSize`**: capacidade total de publicação configurada.
