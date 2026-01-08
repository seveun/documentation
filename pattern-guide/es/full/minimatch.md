# Patrones de URL (Minimatch)

Estos patrones filtran las URLs que el crawler va a explorar. Usan sintaxis **glob** (como wildcards).

---

## Como funciona

En el formulario de configuracion del crawl, tienes 3 campos de filtrado de URL:

| Campo | Comportamiento |
|-------|----------------|
| **Include Patterns** | El crawler SOLO sigue URLs que coinciden con TODOS los patrones |
| **Exclude Patterns** | El crawler ignora URLs que coinciden con AL MENOS UN patron |
| **Subdomains Patterns** | Permite rastrear subdominios especificos |

**Importante**: Estos patrones se aplican al **path** de la URL (despues del dominio), no al dominio.

Cada campo permite multiples patrones (boton +). Introduce un patron por linea.

---

## Sintaxis de patrones

| Patron | Significado | Ejemplo |
|--------|-------------|---------|
| `*` | Cualquier caracter en UN nivel | `/blog/*` coincide con `/blog/articulo` pero NO `/blog/2024/articulo` |
| `**` | Cualquier caracter en MULTIPLES niveles | `/blog/**` coincide con `/blog/articulo` Y `/blog/2024/articulo` |
| `?` | UN solo caracter | `/page?/` coincide con `/page1/` o `/pageA/` |
| `[abc]` | Un caracter de la lista | `/[abc].html` coincide con `/a.html`, `/b.html`, `/c.html` |
| `[!abc]` | Un caracter que NO esta en la lista | `/[!0-9]*` excluye paths que empiezan con digito |

**Opciones**: Coincidencia insensible a mayusculas y parcial activada.

---

## Patrones de inclusion

Con multiples patrones de inclusion, **TODOS deben coincidir** (AND).

| Objetivo | Patron a introducir |
|----------|---------------------|
| Solo el blog | `**/blog/**` |
| Paginas de producto | `**/product/**` |
| Version en espanol | `**/es/**` |
| Categoria especifica | `**/category/zapatos/**` |

---

## Patrones de exclusion

Con multiples patrones de exclusion, **UNO es suficiente** para excluir (OR).

| Objetivo | Patron a introducir |
|----------|---------------------|
| Excluir PDFs | `**/*.pdf` |
| Excluir paginacion | `**/page/[2-9]/**` |
| Excluir paginacion 10+ | `**/page/[1-9][0-9]/**` |
| Excluir busqueda | `**/search/**` |
| Excluir busqueda (param) | `**?s=**` |
| Excluir admin | `**/admin/**` |
| Excluir wp-admin | `**/wp-admin/**` |
| Excluir carrito | `**/cart/**` |
| Excluir checkout | `**/checkout/**` |
| Excluir cuenta | `**/account/**` |
| Excluir ordenamiento | `**?*sort=**` |
| Excluir filtros | `**?*orderby=**` |

---

## Patrones de subdominios

Por defecto, el crawler se queda en el dominio principal. El patron coincide con la parte **ANTES** del dominio.

| Objetivo | Patron a introducir |
|----------|---------------------|
| Subdominio blog | `blog.` |
| Subdominio tienda | `shop.` |
| Todos los subdominios | `*` |

**Nota**: No olvides el punto final (`blog.` no `blog`).

Para idiomas EU, anade un patron por subdominio:
- `es.`
- `en.`
- `fr.`
- `de.`
- `it.`

---

## Entender la logica Include / Exclude

**Importante**: Si defines un Include Pattern, solo las URLs que coincidan seran rastreadas. Todo lo demas queda **automaticamente excluido**.

Los Exclude Patterns solo sirven para:
1. **Afinar** lo que ya esta incluido (ej: blog sin paginacion)
2. **Filtrar un crawl completo** cuando no tienes Include Pattern

---

## Casos de uso

### 1. Auditoria SEO de un blog

**Include Patterns**:
| Patron |
|--------|
| `**/blog/**` |

Eso es todo! Las paginas como `/cart/`, `/admin/`, etc. no coinciden con `**/blog/**` asi que ya estan excluidas.

Para afinar (opcional) - excluir paginacion y paginas de autor del blog:

**Exclude Patterns**:
| Patron |
|--------|
| `**/page/[2-9]/**` |
| `**/page/[1-9][0-9]/**` |
| `**/author/**` |

---

### 2. Auditoria e-commerce - Solo fichas de producto

**Include Patterns**:
| Patron |
|--------|
| `**/product/**` |

Para afinar (opcional) - excluir variantes con filtros:

**Exclude Patterns**:
| Patron |
|--------|
| `**?*sort=**` |
| `**?*filter=**` |

---

### 3. Sitio multilingue - Solo version espanola

**Include Patterns**:
| Patron |
|--------|
| `**/es/**` |

O si usa subdominios:

**Subdomains Patterns**:
| Patron |
|--------|
| `es.` |

---

### 4. Sitio multilingue - Todas las versiones EU

**Subdomains Patterns**:
| Patron |
|--------|
| `es.` |
| `en.` |
| `fr.` |
| `de.` |
| `it.` |
| `pt.` |

---

### 5. Crawl completo EXCEPTO paginas de bajo valor SEO

Cuando quieres rastrear todo el sitio pero excluir ciertas secciones:

**Exclude Patterns**:
| Patron |
|--------|
| `**/search/**` |
| `**/buscar/**` |
| `**?s=**` |
| `**/tag/**` |
| `**/author/**` |
| `**/page/[2-9]/**` |
| `**/page/[1-9][0-9]/**` |
| `**/admin/**` |
| `**/wp-admin/**` |
| `**/cart/**` |
| `**/carrito/**` |
| `**/checkout/**` |
| `**/account/**` |
| `**/login/**` |
| `**/*.pdf` |

---

### 6. Rastrear blog excepto un ano especifico

**Include Patterns**:
| Patron |
|--------|
| `**/blog/**` |

**Exclude Patterns**:
| Patron |
|--------|
| `**/blog/2020/**` |

---

### 7. Rastrear solo paginas de categoria (sin paginacion)

**Include Patterns**:
| Patron |
|--------|
| `**/category/**` |

**Exclude Patterns**:
| Patron |
|--------|
| `**/page/[2-9]/**` |

---

## Errores comunes

| Error | Correccion |
|-------|------------|
| `/blog/` | `**/blog/**` (faltan `**`) |
| `https://site.com/blog/**` | `**/blog/**` (sin dominio) |
| `**/blog` | `**/blog/**` (falta slash final) |
| `blog` (subdominio) | `blog.` (falta punto) |

---

## Pruebas

Prueba tus patrones antes de introducirlos en el front:
- [regex101.com](https://regex101.com/) - Probador de patrones online
