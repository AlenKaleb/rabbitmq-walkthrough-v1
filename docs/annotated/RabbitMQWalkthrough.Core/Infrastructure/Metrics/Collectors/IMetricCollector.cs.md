# RabbitMQWalkthrough.Core/Infrastructure/Metrics/Collectors/IMetricCollector.cs

## Propósito
Contrato para coletores de métricas.

## Código anotado

```csharp
public interface IMetricCollector
{
    Task CollectAndSetAsync(Metric metric);
}
```

### Anotações técnicas
- **Interface única**: facilita DI e composição de coletores.
- **Async por padrão**: permite chamadas a APIs externas (RabbitMQ Management, etc.).
