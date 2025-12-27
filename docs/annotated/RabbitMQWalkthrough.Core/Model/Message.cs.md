# RabbitMQWalkthrough.Core/Model/Message.cs

## Propósito
Modelo de mensagem persistida no banco e serializada para publicação no RabbitMQ.

## Código anotado

```csharp
namespace RabbitMQWalkthrough.Core.Model
{
    public class Message
    {
        public int MessageId { get; set; }

        /// <summary>
        /// Data de Criação da Mensagem
        /// </summary>
        public DateTimeOffset Stored { get; set; }

        /// <summary>
        /// Data de Processamento da Mensagem
        /// </summary>
        public DateTimeOffset? Processed { get; set; }

        public TimeSpan? TimeSpent() => this.Processed?.Subtract(this.Stored) ?? null;
    }
}
```

### Anotações técnicas
- **DTO/Entidade simples**: representa o payload persistido e também enviado ao RabbitMQ.
- **`DateTimeOffset`**: preserva fuso horário e é recomendado para timestamps distribuídos.
- **`TimeSpent()`**: calcula tempo de processamento, útil para métricas e análise de SLA.
