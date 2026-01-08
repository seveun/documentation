# Regex - Hoja de referencia

## Sintaxis

Sin prefijo. Sin `/`. Escapar `\` como `\\` en JSON.

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

```json
// Precio euros
{ "extractPattern": "(\\d+[,.]\\d{2})\\s*€", "extractName": "precio" }

// Telefono ES
{ "extractPattern": "(\\+34[\\s.-]?[6-9]\\d{2}[\\s.-]?\\d{3}[\\s.-]?\\d{3})", "extractName": "tel" }

// Email
{ "extractPattern": "([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,})", "extractName": "email" }

// Codigo postal ES
{ "extractPattern": "\\b(\\d{5})\\b", "extractName": "cp" }

// Fecha DD/MM/AAAA
{ "extractPattern": "(\\d{2}/\\d{2}/\\d{4})", "extractName": "fecha" }

// Tipos schema.org
{ "extractPattern": "\"@type\"\\s*:\\s*\"([^\"]+)\"", "extractName": "schema" }
```

## Errores frecuentes

| Mal | Bien |
|-----|------|
| `/\d+/` | `\\d+` |
| `precio: \d+` | `precio: (\\d+)` (captura) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |

## Prueba

[regex101.com](https://regex101.com) (modo ECMAScript)
