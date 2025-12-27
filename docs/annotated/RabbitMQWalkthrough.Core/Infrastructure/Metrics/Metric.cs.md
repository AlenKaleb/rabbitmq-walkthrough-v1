# RabbitMQWalkthrough.Core/Infrastructure/Metrics/Metric.cs

## Propósito
Modelo de métricas coletadas e persistidas para observabilidade.

## Código anotado

```csharp
public class Metric
{
    public int MetricId { get; set; }
    public DateTime Date { get; set; }
    public int WorkerCount { get; set; }
    public int WorkLoadSize { get; set; }
    public int ConsumerCount { get; set; }
    public int ConsumerThroughput { get; set; }
    public int QueueSize { get; set; }
    public double PublishRate { get; set; }
    public double ConsumeRate { get; set; }
}
```

### Anotações técnicas
- **DTO simples**: reúne métricas de publishers, consumers e fila para análise no Grafana.
- **Campos agregados**: permitem correlacionar workload com taxa de publicação/consumo e tamanho da fila.
