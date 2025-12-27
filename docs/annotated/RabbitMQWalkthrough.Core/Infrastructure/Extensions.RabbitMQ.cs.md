# RabbitMQWalkthrough.Core/Infrastructure/Extensions.RabbitMQ.cs

## Propósito
Extensões utilitárias para simplificar o uso do RabbitMQ.Client.

## Código anotado

```csharp
public static partial class Extensions
{
    public static BasicProperties CreatePersistentBasicProperties(this IChannel channel) =>
        new BasicProperties().SetDeliveryMode(DeliveryModes.Persistent);

    public static BasicProperties SetMessageId(this BasicProperties prop, string messageId)
    {
        prop.MessageId = messageId;
        return prop;
    }

    public static BasicProperties SetCorrelationId(this BasicProperties prop, string correlationId)
    {
        prop.CorrelationId = correlationId;
        return prop;
    }

    public static BasicProperties SetDeliveryMode(this BasicProperties prop, DeliveryModes deliveryMode)
    {
        prop.DeliveryMode = deliveryMode;
        return prop;
    }

    public static async Task<IChannel> SetPrefetchCountAsync(this IChannel channel, ushort prefetchCount)
    {
        await channel.BasicQosAsync(0, prefetchCount, false);
        return channel;
    }
}
```

### Anotações técnicas
- **Extension Methods**: deixam o código de mensageria mais legível e fluido.
- **`DeliveryModes.Persistent`**: garante que mensagens persistentes sobrevivam a reinícios do broker.
- **`SetPrefetchCountAsync`**: controla o *prefetch* para limitar mensagens em voo e proteger consumidores.
- **`MessageId`/`CorrelationId`**: práticas de rastreabilidade e correlação em sistemas distribuídos.
