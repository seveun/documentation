# CSS Selectors - Cheat Sheet

## Syntax

| Selector | Description |
|----------|-------------|
| `h1` | By tag |
| `.class` | By class |
| `#id` | By ID |
| `[attr]` | By attribute |
| `[attr="val"]` | Attribute equals |
| `[attr*="val"]` | Attribute contains |
| `.parent .child` | Descendant |
| `.parent > .child` | Direct child |

## Quick Examples

```json
// Price
{ "extractPattern": ".price", "extractName": "price" }

// Price (partial class)
{ "extractPattern": "[class*=\"price\"]", "extractName": "price" }

// Product title
{ "extractPattern": "h1.product-title", "extractName": "title" }

// Publication date
{ "extractPattern": "time[datetime]", "extractName": "date" }

// SKU via data-attribute
{ "extractPattern": "[data-sku]", "extractName": "sku" }

// Main content
{ "extractPattern": "main", "extractName": "content" }
```

## Common Mistakes

| Wrong | Right |
|-------|-------|
| `.a .b` (space misunderstood) | `.a.b` (same element) or `.a .b` (descendant) |

## Testing

Browser console: `document.querySelectorAll('.your-selector')`
