# WebApplicationEntrypoint/tailwind.config.js

## Propósito
Configura o Tailwind CSS (tema, variantes e plugins).

## Código anotado

```javascript
module.exports = {
    purge: [],
    darkMode: false, // or 'media' or 'class'
    theme: {
        extend: {},
    },
    variants: {
        extend: {},
    },
    plugins: [
        require('@tailwindcss/forms')
    ],
}
```

### Anotações técnicas
- **`purge` vazio**: não remove classes não usadas; em produção, recomenda-se configurar para reduzir o CSS.
- **`@tailwindcss/forms`**: melhora estilos de formulários por padrão.
