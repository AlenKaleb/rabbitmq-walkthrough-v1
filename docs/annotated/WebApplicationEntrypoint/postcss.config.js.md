# WebApplicationEntrypoint/postcss.config.js

## Propósito
Configura o pipeline de processamento de CSS (PostCSS).

## Código anotado

```javascript
module.exports = {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    }
}
```

### Anotações técnicas
- **`tailwindcss`**: gera utilitários CSS a partir do config.
- **`autoprefixer`**: adiciona prefixos de compatibilidade automaticamente.
