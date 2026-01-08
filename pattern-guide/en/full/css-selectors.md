# CSS Selectors

CSS selectors extract specific HTML elements from each crawled page.

---

## How It Works

Enter your CSS selector in the **Extraction Pattern** field. The crawler extracts the **text content** of all matching elements.

---

## Selector Syntax

### Basic Selectors

| Selector | Description | Example |
|----------|-------------|---------|
| `tag` | By tag name | `h1` |
| `.class` | By class | `.product-price` |
| `#id` | By ID | `#main-content` |
| `tag.class` | Tag with class | `h1.title` |
| `.class1.class2` | Element with both classes | `.btn.primary` |

### Attribute Selectors

| Selector | Description | Example |
|----------|-------------|---------|
| `[attr]` | Has attribute | `[data-price]` |
| `[attr="val"]` | Attribute equals value | `[rel="canonical"]` |
| `[attr*="val"]` | Attribute contains | `[class*="price"]` |
| `[attr^="val"]` | Attribute starts with | `[href^="https"]` |
| `[attr$="val"]` | Attribute ends with | `[src$=".jpg"]` |

### Hierarchical Selectors

| Selector | Description | Example |
|----------|-------------|---------|
| `parent child` | Descendant (any level) | `.product .price` |
| `parent > child` | Direct child | `ul > li` |
| `element + next` | Adjacent sibling | `h1 + p` |
| `element ~ siblings` | All following siblings | `h1 ~ p` |

### Useful Pseudo-selectors

| Selector | Description | Example |
|----------|-------------|---------|
| `:first-child` | First child | `li:first-child` |
| `:last-child` | Last child | `li:last-child` |
| `:nth-child(n)` | Nth child | `li:nth-child(2)` |
| `:not(sel)` | Negation | `a:not(.external)` |

---

## Use Cases

### 1. Extract Prices

| Method | Extraction Pattern |
|--------|-------------------|
| By class | `.price` |
| By partial class | `[class*="price"]` |
| In product container | `.product .price` |

### 2. Extract Product Titles

| Method | Extraction Pattern |
|--------|-------------------|
| H1 with class | `h1.product-title` |
| Partial class | `[class*="product-title"]` |

### 3. Extract SKU/References

| Method | Extraction Pattern |
|--------|-------------------|
| Via data-attribute | `[data-sku]` |
| Via class | `.product-reference` |

### 4. Extract Ratings/Reviews

| Method | Extraction Pattern |
|--------|-------------------|
| Partial class | `[class*="rating"]` |
| Exact class | `.review-score` |

### 5. Extract Main Content

| Method | Extraction Pattern |
|--------|-------------------|
| Semantic tag | `main` |
| By ID | `#content` |
| Article tag | `article` |

### 6. Extract Publication Dates

| Method | Extraction Pattern |
|--------|-------------------|
| Time tag | `time[datetime]` |
| By class | `.post-date` |

### 7. Extract Categories

| Method | Extraction Pattern |
|--------|-------------------|
| Breadcrumb | `.breadcrumb a` |
| Partial class | `[class*="category"] a` |

### 8. Extract Authors

| Method | Extraction Pattern |
|--------|-------------------|
| By class | `.author-name` |
| By rel attribute | `[rel="author"]` |

### 9. Extract Product Availability

| Method | Extraction Pattern |
|--------|-------------------|
| Availability | `[class*="availability"]` |
| Stock | `[class*="stock"]` |

### 10. Extract Meta Tags

| Method | Extraction Pattern |
|--------|-------------------|
| Meta description | `meta[name="description"]` |
| Meta robots | `meta[name="robots"]` |

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `.price product` | `.price.product` or `.price .product` |
| `div[class=price]` | `div[class*="price"]` (exact vs contains) |

---

## Tips

### Test Your Selectors

In browser console (F12):
```javascript
document.querySelectorAll('.your-selector')
```

### Robust Selectors

Prefer selectors that won't change:
- `[data-*]` custom attributes
- Semantic tags (`main`, `article`, `nav`)
- Business classes (`product-price` vs `text-red-500`)

### When to Use CSS vs Regex

| Situation | Choice |
|-----------|--------|
| Visible HTML element | CSS |
| Known HTML structure | CSS |
| Text in source code | Regex |
| Complex text pattern | Regex |
| Performance critical | CSS (faster) |
