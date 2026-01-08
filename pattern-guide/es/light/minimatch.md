# Patrones de URL - Hoja de referencia

## Sintaxis

| Patron | Significado |
|--------|-------------|
| `*` | Caracteres en UN nivel |
| `**` | Caracteres en MULTIPLES niveles |
| `?` | UN caracter |
| `[abc]` | Uno de a, b, c |
| `[!abc]` | No a, b, c |

## Campos del formulario

| Campo | Logica |
|-------|--------|
| Include Patterns | Todos deben coincidir (AND) |
| Exclude Patterns | Uno excluye (OR) |
| Subdomains Patterns | Permite subdominios |

## Ejemplos rapidos

**Solo blog** - Include Patterns:
| Patron |
|--------|
| `**/blog/**` |

**Excluir PDFs y paginacion** - Exclude Patterns:
| Patron |
|--------|
| `**/*.pdf` |
| `**/page/[2-9]/**` |

**Subdominios de idioma** - Subdomains Patterns:
| Patron |
|--------|
| `es.` |
| `en.` |
| `fr.` |

**Blog excepto 2020**:

Include Patterns:
| Patron |
|--------|
| `**/blog/**` |

Exclude Patterns:
| Patron |
|--------|
| `**/blog/2020/**` |

## Errores frecuentes

| Mal | Bien |
|-----|------|
| `/blog/` | `**/blog/**` |
| `blog` (subdominio) | `blog.` |
