# RabbitMQWalkthrough.Core/Infrastructure/Queue/PublisherManager.cs

## Propósito
Gerencia múltiplos publishers (criação, listagem e remoção) em tempo de execução.

## Código anotado

```csharp
public class PublisherManager
{
    private readonly IServiceProvider serviceProvider;
    private List<Publisher> publishers = new List<Publisher>();

    public PublisherManager(IServiceProvider serviceProvider)
    {
        this.serviceProvider = serviceProvider;
    }

    public IEnumerable<Publisher> Publishers => this.publishers.ToArray();

    private object syncLock = new Object();

    public void AddPublisher(int size, int messagesPerSecond)
    {
        if (size > 0)
            for (int i = 1; i <= size; i++)
            {
                Publisher publisher = this.serviceProvider.GetRequiredService<Publisher>();
                publisher.Initialize("test_exchange", messagesPerSecond);

                lock (this.syncLock)
                {
                    this.publishers.Add(publisher);
                }

                publisher.Start();
            }
    }

    public void RemovePublisher(string id)
    {
        if (this.publishers.Count > 0)
        {
            lock (this.syncLock)
            {
                Publisher publisher = this.publishers.SingleOrDefault(it => it.Id == id);
                if (publisher != null)
                {
                    this.publishers.Remove(publisher);
                    publisher.Stop();
                }
            }
        }
    }
}
```

### Anotações técnicas
- **Service Locator via `IServiceProvider`**: resolve `Publisher` com DI (dependências injetadas).
- **Lista interna + lock**: sincroniza acesso concorrente de controle (add/remove).
- **`Publishers` exposto como snapshot**: evita mutações externas.
- **Boas práticas**: validação `size > 0` e lifecycle explícito.
