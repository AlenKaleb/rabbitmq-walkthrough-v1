# RabbitMQWalkthrough.Core/Infrastructure/Metrics/Collectors/CollectorFixer.cs

## Propósito
Corrige oscilações em métricas de taxa quando a API do RabbitMQ retorna zeros transitórios.

## Código anotado

```csharp
public class CollectorFixer : IMetricCollector
{
    double lastValidPublishRate = 0;
    double lastValidConsumeRate = 0;
    int zeroMetrics = 0;

    public Task CollectAndSetAsync(Metric metric)
    {
        bool hasMetric = (metric.PublishRate + metric.ConsumeRate > 0);

        if (hasMetric || this.zeroMetrics == 5)
        {
            this.zeroMetrics = 0;
            this.lastValidPublishRate = metric.PublishRate;
            this.lastValidConsumeRate = metric.ConsumeRate;
        }
        else
        {
            this.zeroMetrics++;
            metric.PublishRate = this.lastValidPublishRate;
            metric.ConsumeRate = this.lastValidConsumeRate;
        }

        return Task.CompletedTask;
    }
}
```

### Anotações técnicas
- **Filtro de ruído**: evita que zeros momentâneos afetem gráficos e alertas.
- **Janela de tolerância**: após 5 zeros consecutivos, aceita novo valor como válido.
- **Boas práticas de observabilidade**: estabiliza métricas em ambientes com APIs instáveis.
