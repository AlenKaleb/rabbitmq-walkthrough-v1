# WebApplicationEntrypoint/package.json

## Propósito
Define dependências de front‑end (Tailwind, PostCSS) e scripts npm.

## Código anotado

```json
{
  "name": "webapplicationentrypoint",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {},
  "devDependencies": {
    "@tailwindcss/forms": "^0.5.2",
    "angular": "^1.8.2",
    "autoprefixer": "^10.4.8",
    "postcss": "^8.4.14",
    "tailwindcss": "^3.1.7"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

### Anotações técnicas
- **DevDependencies**: tooling de CSS e UI.
- **`autoprefixer` + `postcss`**: pipeline de CSS para compatibilidade entre navegadores.
- **`tailwindcss`**: framework utilitário para styling.
- **`angular`**: dependência existente, mas não necessariamente usada neste projeto (vale revisar).
