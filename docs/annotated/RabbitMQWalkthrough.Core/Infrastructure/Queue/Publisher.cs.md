# RabbitMQWalkthrough.Core/Infrastructure/Queue/Publisher.cs

## Propósito
Publicador que simula carga de trabalho com taxa configurável e persistência transacional.

## Código anotado

```csharp
public class Publisher
{
    private readonly IChannel channel;
    private readonly IConnection rabbitMqConnection;
    private readonly NpgsqlConnection sqlConnection;
    private readonly MessageDataService messageDataService;
    private readonly ILogger<Publisher> logger;
    private string exchange;
    private readonly Thread runThread;
    private volatile bool isRunning;

    public int MessagesPerSecond { get; private set; }
    public TimeSpan TimeToWait { get; private set; }
    private bool isInitialized;
    public string Id { get; }

    public Publisher(IChannel channel, IConnection rabbitMqConnection, NpgsqlConnection sqlConnection,
        MessageDataService messageDataService, ILogger<Publisher> logger)
    {
        this.channel = channel;
        this.rabbitMqConnection = rabbitMqConnection;
        this.sqlConnection = sqlConnection;
        this.messageDataService = messageDataService;
        this.logger = logger;
        this.Id = Guid.NewGuid().ToString("D");

        this.runThread = new Thread(this.HandlePublishAsync);
    }

    public void Initialize(string exchange, int messagesPerSecond)
    {
        if (this.isInitialized) throw new InvalidOperationException("Initialize só pode ser chamado uma vez");
        this.exchange = exchange;
        this.MessagesPerSecond = messagesPerSecond;
        this.TimeToWait = messagesPerSecond == 0 ? TimeSpan.Zero : this.MessagesPerSecond.AsMessageRateToSleepTimeSpan();
        this.isInitialized = true;
    }

    private void HandlePublishAsync()
    {
        long count = 0;
        while (this.isRunning)
        {
            if (this.MessagesPerSecond != 0)
                this.TimeToWait.Wait();

            count++;

            using NpgsqlTransaction transaction = this.sqlConnection.BeginTransaction();
            try
            {
                Message message = this.messageDataService.CreateMessage(transaction, this.sqlConnection);

                this.channel.BasicPublishAsync(
                    exchange: this.exchange,
                    routingKey: string.Empty,
                    mandatory: true,
                    basicProperties: this.channel.CreatePersistentBasicProperties()
                        .SetMessageId(Guid.NewGuid().ToString("D")),
                    body: message.Serialize().ToByteArray().ToReadOnlyMemory())
                    .GetAwaiter().GetResult();

                transaction.Commit();
            }
            catch (Exception ex)
            {
                transaction.Rollback();
                this.logger.LogError(ex, "Erro ao publicar mensagem. Transação com banco foi abortada.");
            }
        }

        this.channel.CloseAsync().GetAwaiter().GetResult();
        this.channel.Dispose();

        this.rabbitMqConnection.CloseAsync().GetAwaiter().GetResult();
        this.rabbitMqConnection.Dispose();

        this.sqlConnection.Close();
    }

    public Publisher Start()
    {
        if (this.isInitialized == false) throw new InvalidOperationException("Instancia não inicializada");
        this.isRunning = true;
        this.runThread.Start();
        return this;
    }

    public Publisher Stop()
    {
        this.isRunning = false;
        return this;
    }
}
```

### Anotações técnicas
- **Thread dedicada**: simula processamento contínuo e permite taxa constante sem depender de `async`.
- **Taxa configurável**: `MessagesPerSecond` e `TimeToWait` controlam *throughput*.
- **Transação no banco + publicação**: garante consistência entre persistência e envio de mensagens.
- **`BasicPublishAsync` com `Persistent`**: boas práticas de durabilidade do RabbitMQ.
- **Finalização explícita de recursos**: fecha channel, conexão e DB após parar o worker.
