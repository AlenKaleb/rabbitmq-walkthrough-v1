# WebApplicationEntrypoint/Program.cs

## Propósito
Ponto de entrada da aplicação ASP.NET Core.

## Código anotado

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

### Anotações técnicas
- **Host genérico**: `CreateDefaultBuilder` configura logging, configuração e DI padrão.
- **`UseStartup<Startup>`**: define a classe que registra serviços e pipeline HTTP.
