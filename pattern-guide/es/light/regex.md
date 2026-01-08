# Regex - Hoja de referencia

## Sintaxis

Sin prefijo. Sin `/`.

| Patron | Significado |
|--------|-------------|
| `.` | Cualquier caracter |
| `\d` | Digito |
| `\s` | Espacio |
| `\w` | Letra/digito/_ |
| `+` | 1 o mas |
| `*` | 0 o mas |
| `?` | 0 o 1 |
| `{n}` | Exactamente n veces |
| `[abc]` | Uno de |
| `[^abc]` | No |
| `(...)` | Grupo de captura |
| `\b` | Limite de palabra |

## Ejemplos rapidos

| Objetivo | Extraction Pattern |
|----------|-------------------|
| Precio euros | `(\d+[,.]\d{2})\s*€` |
| Telefono ES | `(\+34[\s.-]?[6-9]\d{2}[\s.-]?\d{3}[\s.-]?\d{3})` |
| Email | `([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` |
| Codigo postal ES | `\b(\d{5})\b` |
| Fecha DD/MM/AAAA | `(\d{2}/\d{2}/\d{4})` |
| Tipos schema.org | `"@type"\s*:\s*"([^"]+)"` |

## Errores frecuentes

| Mal | Bien |
|-----|------|
| `/\d+/` | `\d+` |
| `precio: \d+` | `precio: (\d+)` (captura) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |

## Prueba

[regex101.com](https://regex101.com) (modo ECMAScript)
