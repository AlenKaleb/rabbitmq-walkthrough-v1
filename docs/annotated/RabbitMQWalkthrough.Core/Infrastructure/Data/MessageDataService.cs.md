# RabbitMQWalkthrough.Core/Infrastructure/Data/MessageDataService.cs

## Propósito
Encapsula operações SQL para criação e atualização de mensagens no Postgres.

## Código anotado

```csharp
public class MessageDataService
{
    public Message CreateMessage(NpgsqlTransaction transaction, NpgsqlConnection sqlConnection)
    {
        string sql = @"
            INSERT INTO app.\"Messages\" 
                (\"Stored\",\"Num\") 
            VALUES 
                (now(),0) 
            RETURNING *; 
                    ";
        Message message = sqlConnection.QuerySingle<Message>(sql, null, transaction);
        return message;
    }

    public void MarkAsProcessed(Message message, NpgsqlConnection sqlConnection, NpgsqlTransaction sqlTransaction)
    {
        string sql = @"UPDATE app.\"Messages\"
                        SET 
                            \"Processed\" = now(), 
                            \"TimeSpent\" = now() - \"Stored\",
                            \"Num\" = \"Num\" + 1 
                        WHERE 
                            \"MessageId\" = @MessageId ; ";
        
        sqlConnection.Execute(sql, message, sqlTransaction);
    }
}
```

### Anotações técnicas
- **Dapper**: uso direto de SQL facilita controle e performance.
- **Transações externas**: métodos recebem `NpgsqlTransaction`, deixando o controle transacional a cargo do chamador.
- **`RETURNING *`**: retorna a entidade recém‑criada para publicação no RabbitMQ.
- **Atualização de `TimeSpent`**: cálculo no banco garante consistência temporal.
