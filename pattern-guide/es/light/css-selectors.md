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

```json
// Precio
{ "extractPattern": ".price", "extractName": "precio" }

// Precio (clase parcial)
{ "extractPattern": "[class*=\"price\"]", "extractName": "precio" }

// Titulo producto
{ "extractPattern": "h1.product-title", "extractName": "titulo" }

// Fecha publicacion
{ "extractPattern": "time[datetime]", "extractName": "fecha" }

// SKU via data-attribute
{ "extractPattern": "[data-sku]", "extractName": "sku" }

// Contenido principal
{ "extractPattern": "main", "extractName": "contenido" }
```

## Errores frecuentes

| Mal | Bien |
|-----|------|
| `.a .b` (espacio mal entendido) | `.a.b` (mismo elemento) o `.a .b` (descendiente) |

## Prueba

Consola navegador: `document.querySelectorAll('.tu-selector')`
