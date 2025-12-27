# WebApplicationEntrypoint/Views/_ViewStart.cshtml

## Propósito
Define layout padrão para todas as views Razor.

## Código anotado

```cshtml
@{
    Layout = "_Layout";
}
```

### Anotações técnicas
- **Convenção do Razor**: `_ViewStart` aplica layout global sem repetir em cada view.
