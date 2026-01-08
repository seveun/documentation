# Patrones de URL (Minimatch)

Estos patrones filtran las URLs que el crawler va a explorar. Usan sintaxis **glob** (como wildcards).

---

## Como funciona

- **includePatterns**: El crawler SOLO sigue URLs que coinciden con TODOS los patrones
- **excludePatterns**: El crawler ignora URLs que coinciden con AL MENOS UN patron
- **subdomainsPatterns**: Permite rastrear subdominios especificos

**Importante**: Estos patrones se aplican al **path** de la URL (despues del dominio), no al dominio.

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

| Objetivo | Patron |
|----------|--------|
| Solo el blog | `**/blog/**` |
| Paginas de producto | `**/product/**` |
| Version en espanol | `**/es/**` |
| Categoria especifica | `**/category/zapatos/**` |

---

## Patrones de exclusion

Con multiples patrones de exclusion, **UNO es suficiente** para excluir (OR).

| Objetivo | Patron |
|----------|--------|
| Excluir PDFs | `**/*.pdf` |
| Excluir paginacion | `**/page/[2-9]/**`, `**/page/[1-9][0-9]/**` |
| Excluir busqueda | `**/search/**`, `**?s=**` |
| Excluir admin | `**/admin/**`, `**/wp-admin/**` |
| Excluir carrito | `**/cart/**`, `**/checkout/**` |
| Excluir cuenta | `**/account/**`, `**/my-account/**` |
| Excluir ordenamiento | `**?*sort=**`, `**?*orderby=**` |

---

## Patrones de subdominios

Por defecto, el crawler se queda en el dominio principal. El patron coincide con la parte **ANTES** del dominio.

| Objetivo | Patron |
|----------|--------|
| Subdominio blog | `blog.` |
| Subdominio tienda | `shop.` |
| Idiomas EU | `["es.", "en.", "fr.", "de.", "it."]` |
| Todos los subdominios | `*` |

**Nota**: No olvides el punto final (`blog.` no `blog`).

---

## Casos de uso

### 1. Auditoria SEO de un blog

Rastrear solo el blog excluyendo paginas de bajo valor.

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": [
    "**/author/**",
    "**/tag/**",
    "**/category/**",
    "**/page/[2-9]/**",
    "**/page/[1-9][0-9]/**"
  ]
}
```

### 2. Auditoria e-commerce

Rastrear fichas de producto excluyendo el proceso de compra.

```json
{
  "includePatterns": ["**/product/**"],
  "excludePatterns": [
    "**/cart/**",
    "**/checkout/**",
    "**/account/**",
    "**/wishlist/**",
    "**?*sort=**",
    "**?*filter=**"
  ]
}
```

### 3. Sitio multilingue - Solo version espanola

```json
{
  "includePatterns": ["**/es/**"]
}
```

O si usa subdominios:

```json
{
  "subdomainsPatterns": ["es."]
}
```

### 4. Sitio multilingue - Todas las versiones EU

```json
{
  "subdomainsPatterns": ["es.", "en.", "fr.", "de.", "it.", "pt."]
}
```

### 5. Excluir todas las paginas de bajo valor SEO

```json
{
  "excludePatterns": [
    "**/search/**",
    "**/buscar/**",
    "**?s=**",
    "**/tag/**",
    "**/tags/**",
    "**/author/**",
    "**/autor/**",
    "**/page/[2-9]/**",
    "**/page/[1-9][0-9]/**",
    "**/admin/**",
    "**/wp-admin/**",
    "**/cart/**",
    "**/carrito/**",
    "**/checkout/**",
    "**/account/**",
    "**/cuenta/**",
    "**/login/**",
    "**/*.pdf",
    "**/*.zip"
  ]
}
```

### 6. Rastrear blog excepto un ano especifico

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

### 7. Rastrear solo paginas de categoria

```json
{
  "includePatterns": ["**/category/**"],
  "excludePatterns": ["**/page/[2-9]/**"]
}
```

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

Prueba tus patrones en:
- [Globster.xyz](https://globster.xyz/) para probar patrones glob
- Consola del navegador con URLs de ejemplo
