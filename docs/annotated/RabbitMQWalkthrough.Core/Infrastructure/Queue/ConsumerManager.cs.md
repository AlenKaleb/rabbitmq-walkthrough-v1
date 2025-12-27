# RabbitMQWalkthrough.Core/Infrastructure/Queue/ConsumerManager.cs

## Propósito
Gerencia múltiplos consumidores, permitindo escalar o consumo dinamicamente.

## Código anotado

```csharp
public class ConsumerManager
{
    private readonly IServiceProvider serviceProvider;
    private List<Consumer> consumers = new List<Consumer>();

    public ConsumerManager(IServiceProvider serviceProvider)
    {
        this.serviceProvider = serviceProvider;
    }

    public IEnumerable<Consumer> Consumers => this.consumers.ToArray();

    private object syncLock = new();

    public async Task AddConsumerAsync(int size, int messagesPerSecond)
    {
        if (size > 0)
            for (int i = 1; i <= size; i++)
            {
                Consumer consumer = this.serviceProvider.GetRequiredService<Consumer>();
                consumer.Initialize("test_queue", messagesPerSecond);
                lock (this.syncLock)
                {
                    this.consumers.Add(consumer);
                }
                await consumer.StartAsync();
            }
    }

    public void RemoveConsumer(string id)
    {
        if (this.consumers.Count > 0)
        {
            lock (this.syncLock)
            {
                Consumer consumer = this.consumers.SingleOrDefault(it => it.Id == id);
                if (consumer != null)
                {
                    this.consumers.Remove(consumer);
                    consumer.Stop();
                }
            }
        }
    }
}
```

### Anotações técnicas
- **Escala horizontal local**: cria múltiplas instâncias para simular concorrência.
- **Sincronização com lock**: protege a lista compartilhada contra concorrência.
- **Lifecycle explícito**: `StartAsync`/`Stop` mantêm o estado consistente.
