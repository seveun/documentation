# Selectores CSS

Los selectores CSS extraen elementos HTML especificos de cada pagina rastreada.

---

## Como funciona

Introduce tu selector CSS en el campo **Extraction Pattern**. El crawler extrae el **texto** de todos los elementos que coinciden.

---

## Sintaxis de selectores

### Selectores basicos

| Selector | Descripcion | Ejemplo |
|----------|-------------|---------|
| `tag` | Por nombre de etiqueta | `h1` |
| `.clase` | Por clase | `.product-price` |
| `#id` | Por ID | `#main-content` |
| `tag.clase` | Etiqueta con clase | `h1.title` |
| `.clase1.clase2` | Elemento con ambas clases | `.btn.primary` |

### Selectores de atributos

| Selector | Descripcion | Ejemplo |
|----------|-------------|---------|
| `[attr]` | Tiene el atributo | `[data-price]` |
| `[attr="val"]` | Atributo igual a valor | `[rel="canonical"]` |
| `[attr*="val"]` | Atributo contiene | `[class*="price"]` |
| `[attr^="val"]` | Atributo empieza con | `[href^="https"]` |
| `[attr$="val"]` | Atributo termina con | `[src$=".jpg"]` |

### Selectores jerarquicos

| Selector | Descripcion | Ejemplo |
|----------|-------------|---------|
| `padre hijo` | Descendiente (cualquier nivel) | `.product .price` |
| `padre > hijo` | Hijo directo | `ul > li` |
| `elemento + siguiente` | Hermano adyacente | `h1 + p` |
| `elemento ~ hermanos` | Todos los hermanos siguientes | `h1 ~ p` |

### Pseudo-selectores utiles

| Selector | Descripcion | Ejemplo |
|----------|-------------|---------|
| `:first-child` | Primer hijo | `li:first-child` |
| `:last-child` | Ultimo hijo | `li:last-child` |
| `:nth-child(n)` | N-esimo hijo | `li:nth-child(2)` |
| `:not(sel)` | Negacion | `a:not(.external)` |

---

## Casos de uso

### 1. Extraer precios

| Metodo | Extraction Pattern |
|--------|-------------------|
| Por clase | `.price` |
| Por clase parcial | `[class*="price"]` |
| Precio en contenedor | `.product .price` |

### 2. Extraer titulos de productos

| Metodo | Extraction Pattern |
|--------|-------------------|
| H1 con clase | `h1.product-title` |
| Clase parcial | `[class*="product-title"]` |

### 3. Extraer SKU/Referencias

| Metodo | Extraction Pattern |
|--------|-------------------|
| Via data-attribute | `[data-sku]` |
| Via clase | `.product-reference` |

### 4. Extraer puntuaciones/resenas

| Metodo | Extraction Pattern |
|--------|-------------------|
| Clase parcial | `[class*="rating"]` |
| Clase exacta | `.review-score` |

### 5. Extraer contenido principal

| Metodo | Extraction Pattern |
|--------|-------------------|
| Etiqueta semantica | `main` |
| Por ID | `#content` |
| Etiqueta article | `article` |

### 6. Extraer fechas de publicacion

| Metodo | Extraction Pattern |
|--------|-------------------|
| Etiqueta time | `time[datetime]` |
| Por clase | `.post-date` |

### 7. Extraer categorias

| Metodo | Extraction Pattern |
|--------|-------------------|
| Breadcrumb | `.breadcrumb a` |
| Clase parcial | `[class*="category"] a` |

### 8. Extraer autores

| Metodo | Extraction Pattern |
|--------|-------------------|
| Por clase | `.author-name` |
| Por atributo rel | `[rel="author"]` |

### 9. Extraer disponibilidad de producto

| Metodo | Extraction Pattern |
|--------|-------------------|
| Disponibilidad | `[class*="availability"]` |
| Stock | `[class*="stock"]` |

### 10. Extraer meta tags

| Metodo | Extraction Pattern |
|--------|-------------------|
| Meta description | `meta[name="description"]` |
| Meta robots | `meta[name="robots"]` |

---

## Errores comunes

| Error | Correccion |
|-------|------------|
| `.price product` | `.price.product` o `.price .product` |
| `div[class=price]` | `div[class*="price"]` (exacto vs contiene) |

---

## Consejos

### Probar tus selectores

En la consola del navegador (F12):
```javascript
document.querySelectorAll('.tu-selector')
```

### Selectores robustos

Prefiere selectores que no cambien:
- `[data-*]` atributos custom
- Etiquetas semanticas (`main`, `article`, `nav`)
- Clases de negocio (`product-price` vs `text-red-500`)

### Cuando usar CSS vs Regex

| Situacion | Eleccion |
|-----------|----------|
| Elemento HTML visible | CSS |
| Estructura HTML conocida | CSS |
| Texto en codigo fuente | Regex |
| Patron complejo en texto | Regex |
| Rendimiento critico | CSS (mas rapido) |
