# WebApplicationEntrypoint/Views/_ViewImports.cshtml

## Propósito
Importações compartilhadas por todas as views Razor.

## Código anotado

```cshtml
@using WebApplicationEntrypoint
@using WebApplicationEntrypoint.Models
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

### Anotações técnicas
- **`@using`**: reduz repetição de namespaces nas views.
- **TagHelpers**: habilita helpers HTML do ASP.NET Core.
