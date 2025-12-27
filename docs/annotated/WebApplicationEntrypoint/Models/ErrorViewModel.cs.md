# WebApplicationEntrypoint/Models/ErrorViewModel.cs

## Propósito
ViewModel para a página de erro.

## Código anotado

```csharp
public class ErrorViewModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(this.RequestId);
}
```

### Anotações técnicas
- **Seletor de exibição**: `ShowRequestId` evita renderizar IDs vazios.
- **Separação de responsabilidades**: view model simples mantém a view limpa.
