# RabbitMQWalkthrough.Core/Infrastructure/Metrics/Collectors/ConsumerMetricCollector.cs

## Propósito
Coleta métricas internas de consumidores (quantidade e throughput configurado).

## Código anotado

```csharp
public class ConsumerMetricCollector : IMetricCollector
{
    private readonly ConsumerManager consumerManager;

    public ConsumerMetricCollector(ConsumerManager consumerManager)
    {
        this.consumerManager = consumerManager;
    }

    public Task CollectAndSetAsync(Metric metric)
    {
        metric.ConsumerCount = this.consumerManager.Consumers.Count();
        metric.ConsumerThroughput = this.consumerManager.Consumers.Sum(it => it.MessagesPerSecond);

        return Task.CompletedTask;
    }
}
```

### Anotações técnicas
- **Métricas derivadas**: soma de `MessagesPerSecond` fornece a capacidade configurada de consumo.
- **Baixo custo**: coleta local sem I/O externo.
