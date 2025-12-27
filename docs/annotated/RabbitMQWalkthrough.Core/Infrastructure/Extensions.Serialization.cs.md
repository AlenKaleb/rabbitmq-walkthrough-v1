# RabbitMQWalkthrough.Core/Infrastructure/Extensions.Serialization.cs

## Propósito
Centraliza serialização JSON e conversões UTF‑8 para simplificar publicação/consumo.

## Código anotado

```csharp
public static partial class Extensions
{
    public static string Serialize<T>(this T objectToSerialize) =>
        System.Text.Json.JsonSerializer.Serialize<T>(objectToSerialize);

    public static T Deserialize<T>(this string jsonText) =>
        System.Text.Json.JsonSerializer.Deserialize<T>(jsonText);

    public static byte[] ToByteArray(this string text) =>
        System.Text.Encoding.UTF8.GetBytes(text);

    public static string ToUTF8String(this byte[] bytes) =>
        System.Text.Encoding.UTF8.GetString(bytes);

    public static ReadOnlyMemory<byte> ToReadOnlyMemory(this byte[] bytes) =>
        new ReadOnlyMemory<byte>(bytes);
}
```

### Anotações técnicas
- **`System.Text.Json`**: serializer moderno e eficiente para JSON.
- **Conversões UTF‑8**: padrão para interoperabilidade em mensageria e HTTP.
- **`ReadOnlyMemory<byte>`**: evita cópias desnecessárias ao publicar no RabbitMQ.
