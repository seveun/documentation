# Patrones de URL - Hoja de referencia

## Sintaxis

| Patron | Significado |
|--------|-------------|
| `*` | Caracteres en UN nivel |
| `**` | Caracteres en MULTIPLES niveles |
| `?` | UN caracter |
| `[abc]` | Uno de a, b, c |
| `[!abc]` | No a, b, c |

## Opciones

| Opcion | Logica | Ejemplo |
|--------|--------|---------|
| `includePatterns` | Todos deben coincidir (AND) | `["**/blog/**"]` |
| `excludePatterns` | Uno excluye (OR) | `["**/*.pdf"]` |
| `subdomainsPatterns` | Permite subdominios | `["blog.", "shop."]` |

## Ejemplos rapidos

```json
// Solo blog
{ "includePatterns": ["**/blog/**"] }

// Excluir PDFs y paginacion
{ "excludePatterns": ["**/*.pdf", "**/page/[2-9]/**"] }

// Subdominios de idioma
{ "subdomainsPatterns": ["es.", "en.", "fr."] }

// Blog excepto 2020
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

## Errores frecuentes

| Mal | Bien |
|-----|------|
| `/blog/` | `**/blog/**` |
| `blog` (subdominio) | `blog.` |
