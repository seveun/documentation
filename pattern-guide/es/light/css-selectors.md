# Selectores CSS - Hoja de referencia

## Sintaxis

| Selector | Descripcion |
|----------|-------------|
| `h1` | Por etiqueta |
| `.clase` | Por clase |
| `#id` | Por ID |
| `[attr]` | Por atributo |
| `[attr="val"]` | Atributo igual |
| `[attr*="val"]` | Atributo contiene |
| `.padre .hijo` | Descendiente |
| `.padre > .hijo` | Hijo directo |

## Ejemplos rapidos

| Objetivo | Extraction Pattern |
|----------|-------------------|
| Precio | `.price` |
| Precio (clase parcial) | `[class*="price"]` |
| Titulo producto | `h1.product-title` |
| Fecha publicacion | `time[datetime]` |
| SKU | `[data-sku]` |
| Contenido principal | `main` |
| Categorias | `.breadcrumb a` |
| Autor | `.author-name` |

## Errores frecuentes

| Mal | Bien |
|-----|------|
| `.a .b` (espacio mal entendido) | `.a.b` (mismo elemento) o `.a .b` (descendiente) |

## Prueba

Consola navegador: `document.querySelectorAll('.tu-selector')`
