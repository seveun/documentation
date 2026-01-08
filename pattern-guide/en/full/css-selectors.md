# CSS Selectors

CSS selectors extract specific HTML elements from each crawled page.

---

## How It Works

Enter your CSS selector in the extraction field.

```
extractPattern: ".product-price"
extractName: "price"
```

The crawler extracts the **text content** of all matching elements.

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

**By class:**
```json
{
  "extractPattern": ".price",
  "extractName": "price"
}
```

**By partial class:**
```json
{
  "extractPattern": "[class*=\"price\"]",
  "extractName": "price"
}
```

**Price in product container:**
```json
{
  "extractPattern": ".product .price",
  "extractName": "price"
}
```

### 2. Extract Product Titles

```json
{
  "extractPattern": "h1.product-title",
  "extractName": "product_title"
}
```

Or more generic:
```json
{
  "extractPattern": "[class*=\"product-title\"]",
  "extractName": "product_title"
}
```

### 3. Extract SKU/References

**Via data-attribute:**
```json
{
  "extractPattern": "[data-sku]",
  "extractName": "sku"
}
```

**Via class:**
```json
{
  "extractPattern": ".product-reference",
  "extractName": "reference"
}
```

### 4. Extract Ratings/Reviews

```json
{
  "extractPattern": "[class*=\"rating\"]",
  "extractName": "rating"
}
```

Or:
```json
{
  "extractPattern": ".review-score",
  "extractName": "rating"
}
```

### 5. Extract Main Content

**Via semantic tag:**
```json
{
  "extractPattern": "main",
  "extractName": "content"
}
```

**Via ID:**
```json
{
  "extractPattern": "#content",
  "extractName": "content"
}
```

**Via article tag:**
```json
{
  "extractPattern": "article",
  "extractName": "content"
}
```

### 6. Extract Publication Dates

**Via time tag:**
```json
{
  "extractPattern": "time[datetime]",
  "extractName": "publish_date"
}
```

**Via class:**
```json
{
  "extractPattern": ".post-date",
  "extractName": "publish_date"
}
```

### 7. Extract Categories

```json
{
  "extractPattern": ".breadcrumb a",
  "extractName": "categories"
}
```

Or:
```json
{
  "extractPattern": "[class*=\"category\"] a",
  "extractName": "categories"
}
```

### 8. Extract Authors

```json
{
  "extractPattern": ".author-name",
  "extractName": "author"
}
```

Or:
```json
{
  "extractPattern": "[rel=\"author\"]",
  "extractName": "author"
}
```

### 9. Extract Product Availability

```json
{
  "extractPattern": "[class*=\"availability\"]",
  "extractName": "availability"
}
```

Or:
```json
{
  "extractPattern": "[class*=\"stock\"]",
  "extractName": "stock"
}
```

### 10. Extract Meta Tags

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
