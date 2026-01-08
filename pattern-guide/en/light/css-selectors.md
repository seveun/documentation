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

| Purpose | Extraction Pattern |
|---------|-------------------|
| Price | `.price` |
| Price (partial class) | `[class*="price"]` |
| Product title | `h1.product-title` |
| Publication date | `time[datetime]` |
| SKU | `[data-sku]` |
| Main content | `main` |
| Categories | `.breadcrumb a` |
| Author | `.author-name` |

## Common Mistakes

| Wrong | Right |
|-------|-------|
| `.a .b` (space misunderstood) | `.a.b` (same element) or `.a .b` (descendant) |

## Testing

Browser console: `document.querySelectorAll('.your-selector')`
