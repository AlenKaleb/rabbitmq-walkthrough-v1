# WebApplicationEntrypoint/Controllers/ConsumerController.cs

## Propósito
API REST para criar/remover consumidores e consultar estado atual.

## Código anotado

```csharp
[Route("api/[controller]")]
[ApiController]
public class ConsumerController : ControllerBase
{
    private readonly ConsumerManager consumerManager;

    public ConsumerController(ConsumerManager consumerManager)
    {
        this.consumerManager = consumerManager;
    }

    [HttpPut]
    public async Task AddConsumerAsync([FromQuery] int size, [FromQuery] int messagesPerSecond)
    {
        await this.consumerManager.AddConsumerAsync(size, messagesPerSecond);
    }

    [HttpDelete("{id}")]
    public void RemoveConsumer(string id)
    {
        this.consumerManager.RemoveConsumer(id);
    }

    [HttpGet()]
    public IEnumerable<Consumer> GetConsumer()
    {
        return this.consumerManager.Consumers;
    }
}
```

### Anotações técnicas
- **Endpoints idempotentes**: `PUT` para criar e configurar consumidores.
- **Assíncrono**: `AddConsumerAsync` inicia consumidores sem bloquear a thread de requisição.
- **Consulta de estado**: `GET` expõe consumidores ativos para UI/monitoramento.
