# RabbitMQWalkthrough.Core/Infrastructure/Extensions.Generic.cs

## Propósito
Fornece utilitários genéricos: controle de taxa de mensagens e registro de serviços com *retry*.

## Código anotado

```csharp
public static partial class Extensions
{
    public static TimeSpan AsMessageRateToSleepTimeSpan(this int messagesPerSecond)
    {
        if (messagesPerSecond < 1)
            throw new ArgumentOutOfRangeException(nameof(messagesPerSecond));

        long ticksPerSecond = TimeSpan.FromSeconds(1).Ticks;
        int sleepTimer = Convert.ToInt32((ticksPerSecond / messagesPerSecond) - (messagesPerSecond / 12 ));
        return TimeSpan.FromTicks(Math.Max(sleepTimer, 0));
    }

    public static IServiceCollection AddTransientWithRetry<TService, TKnowException>(
        this IServiceCollection services,
        Func<IServiceProvider, TService> implementationFactory)
        where TKnowException : Exception
        where TService : class
    {
        return services.AddTransient(sp =>
        {
            TService returnValue = default;
            RetryPolicy policy = BuildPolicy<TKnowException>();
            policy.Execute(() =>
            {
                returnValue = implementationFactory(sp);
            });
            return returnValue;
        });
    }

    public static IServiceCollection AddSingletonWithRetry<TService, TKnowException>(
        this IServiceCollection services,
        Func<IServiceProvider, TService> implementationFactory)
        where TKnowException : Exception
        where TService : class
    {
        return services.AddSingleton(sp =>
        {
            TService returnValue = default;
            BuildPolicy<TKnowException>().Execute(() => { returnValue = implementationFactory(sp); });
            return returnValue;
        });
    }

    public static IServiceCollection AddScopedWithRetry<TService, TKnowException>(
        this IServiceCollection services,
        Func<IServiceProvider, TService> implementationFactory)
        where TKnowException : Exception
        where TService : class
    {
        return services.AddScoped(sp =>
        {
            TService returnValue = default;
            BuildPolicy<TKnowException>().Execute(() => { returnValue = implementationFactory(sp); });
            return returnValue;
        });
    }

    private static RetryPolicy BuildPolicy<TKnowException>(int retryCount = 5)
        where TKnowException : Exception
    {
        return Policy
            .Handle<TKnowException>()
            .WaitAndRetry(retryCount, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));
    }
}
```

### Anotações técnicas
- **Throttle por taxa**: converte `messagesPerSecond` em um `TimeSpan` para controlar *throughput* em publishers/consumers.
- **Algoritmo de sleep ajustado**: a fórmula tenta compensar overhead de loop e thread, aproximando a taxa desejada.
- **Registro com retry (Polly)**: encapsula resiliência na criação de conexões/transientes, reduzindo falhas por indisponibilidade momentânea.
- **`BuildPolicy` com backoff exponencial**: padrão para estabilizar serviços dependentes.
