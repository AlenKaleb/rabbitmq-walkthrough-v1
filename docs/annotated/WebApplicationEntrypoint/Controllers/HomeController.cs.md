# WebApplicationEntrypoint/Controllers/HomeController.cs

## Propósito
Controlador MVC padrão para páginas Razor.

## Código anotado

```csharp
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        this._logger = logger;
    }

    public IActionResult Index()
    {
        return this.View();
    }

    public IActionResult Privacy()
    {
        return this.View();
    }

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public IActionResult Error()
    {
        return this.View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? this.HttpContext.TraceIdentifier });
    }
}
```

### Anotações técnicas
- **MVC tradicional**: expõe views Razor para páginas básicas.
- **`ResponseCache`**: desabilita cache da página de erro para evitar informações desatualizadas.
- **`ErrorViewModel`**: centraliza o `RequestId` para troubleshooting.
