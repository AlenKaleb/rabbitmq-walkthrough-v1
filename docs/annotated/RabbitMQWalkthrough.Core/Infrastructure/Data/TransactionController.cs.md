# RabbitMQWalkthrough.Core/Infrastructure/Data/TransactionController.cs

## Propósito
Abstrai a execução de operações sob transação com commit/rollback e logging.

## Código anotado

```csharp
public class TransactionController
{
    private readonly ILogger<TransactionController> logger;
    private readonly IServiceProvider serviceProvider;

    public TransactionController(ILogger<TransactionController> logger, IServiceProvider serviceProvider)
    {
        this.logger = logger;
        this.serviceProvider = serviceProvider;
    }

    public void RunUnderTransaction(Action<IServiceProvider> action)
    {
        using IServiceScope scope = this.serviceProvider.CreateScope();
        using SqlTransaction transaction = scope.ServiceProvider.GetRequiredService<SqlTransaction>();
        try
        {
            action(scope.ServiceProvider);
            transaction.Commit();
        }
        catch (Exception ex)
        {
            transaction.Rollback();
            this.logger.LogError(ex, "Erro ao publicar mensagem. Transação com banco foi abortada.");
            throw;
        }
    }
}
```

### Anotações técnicas
- **Escopo de DI (`CreateScope`)**: garante dependências transientes/escopadas isoladas por transação.
- **`Commit`/`Rollback` explícitos**: proteção contra inconsistências.
- **Logging estruturado**: mantém rastreabilidade de falhas.

> Observação: a classe usa `SqlTransaction` (SQL Server). O projeto principal usa Postgres (`Npgsql`). Isso sugere refatoração ou adaptação se o fluxo transacional for usado em produção com Postgres.
