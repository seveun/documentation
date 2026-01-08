# Selectores CSS

Los selectores CSS extraen elementos HTML especificos de cada pagina rastreada.

---

## Como funciona

Introduce tu selector CSS en el campo de extraccion.

```
extractPattern: ".product-price"
extractName: "precio"
```

El crawler extrae el **texto** de todos los elementos que coinciden.

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

**Por clase:**
```json
{
  "extractPattern": ".price",
  "extractName": "precio"
}
```

**Por clase parcial:**
```json
{
  "extractPattern": "[class*=\"price\"]",
  "extractName": "precio"
}
```

**Precio en contenedor de producto:**
```json
{
  "extractPattern": ".product .price",
  "extractName": "precio"
}
```

### 2. Extraer titulos de productos

```json
{
  "extractPattern": "h1.product-title",
  "extractName": "titulo_producto"
}
```

O mas generico:
```json
{
  "extractPattern": "[class*=\"product-title\"]",
  "extractName": "titulo_producto"
}
```

### 3. Extraer SKU/Referencias

**Via data-attribute:**
```json
{
  "extractPattern": "[data-sku]",
  "extractName": "sku"
}
```

**Via clase:**
```json
{
  "extractPattern": ".product-reference",
  "extractName": "referencia"
}
```

### 4. Extraer puntuaciones/resenas

```json
{
  "extractPattern": "[class*=\"rating\"]",
  "extractName": "puntuacion"
}
```

O:
```json
{
  "extractPattern": ".review-score",
  "extractName": "puntuacion"
}
```

### 5. Extraer contenido principal

**Via etiqueta semantica:**
```json
{
  "extractPattern": "main",
  "extractName": "contenido"
}
```

**Via ID:**
```json
{
  "extractPattern": "#content",
  "extractName": "contenido"
}
```

**Via etiqueta article:**
```json
{
  "extractPattern": "article",
  "extractName": "contenido"
}
```

### 6. Extraer fechas de publicacion

**Via etiqueta time:**
```json
{
  "extractPattern": "time[datetime]",
  "extractName": "fecha_publicacion"
}
```

**Via clase:**
```json
{
  "extractPattern": ".post-date",
  "extractName": "fecha_publicacion"
}
```

### 7. Extraer categorias

```json
{
  "extractPattern": ".breadcrumb a",
  "extractName": "categorias"
}
```

O:
```json
{
  "extractPattern": "[class*=\"category\"] a",
  "extractName": "categorias"
}
```

### 8. Extraer autores

```json
{
  "extractPattern": ".author-name",
  "extractName": "autor"
}
```

O:
```json
{
  "extractPattern": "[rel=\"author\"]",
  "extractName": "autor"
}
```

### 9. Extraer disponibilidad de producto

```json
{
  "extractPattern": "[class*=\"availability\"]",
  "extractName": "disponibilidad"
}
```

O:
```json
{
  "extractPattern": "[class*=\"stock\"]",
  "extractName": "stock"
}
```

### 10. Extraer meta tags

**Meta description:**
```json
{
  "extractPattern": "meta[name=\"description\"]",
  "extractName": "meta_description"
}
```

**Meta robots:**
```json
{
  "extractPattern": "meta[name=\"robots\"]",
  "extractName": "meta_robots"
}
```

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
