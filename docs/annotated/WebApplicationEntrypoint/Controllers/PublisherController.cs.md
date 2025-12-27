# WebApplicationEntrypoint/Controllers/PublisherController.cs

## Propósito
API REST para criar/remover publishers e consultar o estado atual.

## Código anotado

```csharp
[Route("api/[controller]")]
[ApiController]
public class PublisherController : ControllerBase
{
    private readonly PublisherManager publisherManager;

    public PublisherController(PublisherManager publisherManager)
    {
        this.publisherManager = publisherManager;
    }

    [HttpPut]
    public void AddPublisher([FromQuery]int size, [FromQuery] int messagesPerSecond)
    {
        this.publisherManager.AddPublisher(size, messagesPerSecond);
    }

    [HttpDelete("{id}")]
    public void RemovePublisher(string id)
    {
        this.publisherManager.RemovePublisher(id);
    }

    [HttpGet()]
    public IEnumerable<Publisher> GetPublishers()
    {
        return this.publisherManager.Publishers;
    }
}
```

### Anotações técnicas
- **API simples**: `PUT` cria publishers, `DELETE` remove e `GET` lista.
- **Parâmetros via query**: permite controlar rapidamente tamanho e taxa.
- **Responsabilidade única**: delega a criação/remoção ao `PublisherManager`.
