# RabbitMQWalkthrough.Core/Infrastructure/Extensions.Wait.cs

## Propósito
Fornece um método de espera simples usando `Thread.Sleep`.

## Código anotado

```csharp
public static partial class Extensions
{
    public static void Wait(this TimeSpan time)
    {
        System.Threading.Thread.Sleep(time);
    }
}
```

### Anotações técnicas
- **Simplificação semântica**: permite escrever `timeSpan.Wait()` para legibilidade.
- **Thread.Sleep**: bloqueia a thread; adequado para simulação de carga, mas não recomendado em fluxos assíncronos.
