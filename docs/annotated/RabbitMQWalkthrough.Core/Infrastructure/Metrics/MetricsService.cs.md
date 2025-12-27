# RabbitMQWalkthrough.Core/Infrastructure/Metrics/MetricsService.cs

## Propósito
Orquestra a coleta de métricas e sua persistência no banco.

## Código anotado

```csharp
public class MetricsService
{
    private readonly IEnumerable<IMetricCollector> metricCollectors;
    private readonly NpgsqlConnection sqlConnection;

    public MetricsService(IEnumerable<IMetricCollector> metricCollectors, NpgsqlConnection sqlConnection)
    {
        this.metricCollectors = metricCollectors;
        this.sqlConnection = sqlConnection;
    }

    public async Task CollectAndStoreAsync() => await this.StoreAsync(await this.CollectAsync());

    private async Task<Metric> CollectAsync()
    {
        Metric metric = new()
        {
            Date = DateTime.UtcNow
        };

        foreach (IMetricCollector metricCollector in this.metricCollectors)
        {
            await metricCollector.CollectAndSetAsync(metric);
        }

        return metric;
    }

    private async Task StoreAsync(Metric metric)
    {
        await this.sqlConnection.ExecuteAsync(@"INSERT INTO app.\"Metrics\"
       (\"Date\"
       ,\"WorkerCount\"
       ,\"WorkLoadSize\"
       ,\"ConsumerCount\"
       ,\"ConsumerThroughput\"
       ,\"QueueSize\"
       ,\"PublishRate\"
       ,\"ConsumeRate\")
 VALUES
       (@Date
       ,@WorkerCount
       ,@WorkLoadSize
       ,@ConsumerCount
       ,@ConsumerThroughput
       ,@QueueSize
       ,@PublishRate
       ,@ConsumeRate)", metric);
    }
}
```

### Anotações técnicas
- **Strategy/Plugin**: `IMetricCollector` permite adicionar novas fontes de métricas sem alterar o serviço.
- **Pipeline simples**: coleta em memória e persiste em sequência.
- **Dapper com SQL explícito**: mantém controle do esquema e eficiência.
