# Regular Expressions (Regex)

Regex extracts text by matching patterns in the HTML source code.

---

## How It Works

Write your regex directly in the **Extraction Pattern** field (no prefix).

**Important**:
- Do NOT wrap with `/`
- Use **capture groups** `()` to extract specific parts
- The `g` flag (global) is applied automatically

---

## Basic Syntax

### Special Characters

| Pattern | Meaning | Example |
|---------|---------|---------|
| `.` | Any character | `a.c` → "abc", "a1c" |
| `\d` | A digit (0-9) | `\d+` → "123" |
| `\D` | Not a digit | `\D+` → "abc" |
| `\w` | Letter, digit or _ | `\w+` → "hello_123" |
| `\W` | Not a word char | `\W+` → "!@#" |
| `\s` | Space, tab, newline | `hello\s+world` |
| `\S` | Not a space | `\S+` → "hello" |

### Quantifiers

| Pattern | Meaning | Example |
|---------|---------|---------|
| `*` | 0 or more | `ab*c` → "ac", "abc", "abbc" |
| `+` | 1 or more | `ab+c` → "abc", "abbc" (not "ac") |
| `?` | 0 or 1 | `colou?r` → "color", "colour" |
| `{n}` | Exactly n times | `\d{5}` → "12345" |
| `{n,}` | At least n times | `\d{2,}` → "12", "123", "1234" |
| `{n,m}` | Between n and m | `\d{2,4}` → "12", "123", "1234" |

### Character Classes

| Pattern | Meaning | Example |
|---------|---------|---------|
| `[abc]` | One of a, b, c | `[aeiou]` → vowels |
| `[^abc]` | Not a, b, c | `[^0-9]` → not a digit |
| `[a-z]` | Range | `[a-zA-Z]` → letters |
| `[0-9]` | Same as `\d` | `[0-9]+` → numbers |

### Anchors and Boundaries

| Pattern | Meaning | Example |
|---------|---------|---------|
| `^` | Start of line | `^Title` |
| `$` | End of line | `\.html$` |
| `\b` | Word boundary | `\bprice\b` → "price" alone |

### Groups

| Pattern | Meaning | Example |
|---------|---------|---------|
| `(...)` | Capture group | `price: (\d+)` → captures number |
| `(?:...)` | Non-capture group | `(?:www\.)?` |
| `\|` | Alternative (OR) | `(cat\|dog)` |

---

## Use Cases

### 1. Extract Prices

| Method | Extraction Pattern |
|--------|-------------------|
| US format ($29.99) | `\$(\d+\.\d{2})` |
| European format (29,99 €) | `(\d+[,.]\d{2})\s*€` |
| With EUR | `(\d+[,.]\d{2})\s*EUR` |

### 2. Extract Phone Numbers

| Method | Extraction Pattern |
|--------|-------------------|
| US format | `(\d{3}[-.\\s]?\d{3}[-.\\s]?\d{4})` |
| UK format | `(0\d{2,4}[\s.-]?\d{3,4}[\s.-]?\d{3,4})` |
| International | `(\+1[\s.-]?\d{3}[\s.-]?\d{3}[\s.-]?\d{4})` |

Matches: "555-123-4567", "5551234567", "555.123.4567"

### 3. Extract Emails

| Method | Extraction Pattern |
|--------|-------------------|
| Standard email | `([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` |

### 4. Extract Zip Codes

| Method | Extraction Pattern |
|--------|-------------------|
| US (5 digits or 5+4) | `\b(\d{5}(?:-\d{4})?)\b` |
| UK | `([A-Z]{1,2}\d[A-Z\d]?\s?\d[A-Z]{2})` |

### 5. Extract Dates

| Method | Extraction Pattern |
|--------|-------------------|
| MM/DD/YYYY format | `(\d{2}/\d{2}/\d{4})` |
| YYYY-MM-DD format | `(\d{4}-\d{2}-\d{2})` |
| Text format | `((?:January\|February\|March\|April\|May\|June\|July\|August\|September\|October\|November\|December)\s+\d{1,2},?\s+\d{4})` |

### 6. Extract Product References

| Method | Extraction Pattern |
|--------|-------------------|
| Format ABC-12345 | `([A-Z]{2,4}-\d{4,6})` |
| EAN/GTIN format | `\b(\d{13})\b` |

### 7. Extract Structured Data

| Method | Extraction Pattern |
|--------|-------------------|
| Schema.org types | `"@type"\s*:\s*"([^"]+)"` |

### 8. Extract Specific Meta Tags

| Method | Extraction Pattern |
|--------|-------------------|
| Meta author | `<meta[^>]*name=["']author["'][^>]*content=["']([^"']+)["']` |
| Meta robots | `<meta[^>]*name=["']robots["'][^>]*content=["']([^"']+)["']` |

### 9. Extract Data Attributes

| Method | Extraction Pattern |
|--------|-------------------|
| data-product-id | `data-product-id=["'](\d+)["']` |
| data-price | `data-price=["']([\d.,]+)["']` |

### 10. Extract URLs

| Method | Extraction Pattern |
|--------|-------------------|
| Image URLs | `<img[^>]+src=["']([^"']+)["']` |
| Link URLs | `<a[^>]+href=["']([^"']+)["']` |

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `/\d+/` | `\d+` (no slashes) |
| `price: \d+` | `price: (\d+)` (capture group) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |
| `10.99` | `10\.99` (escape dot) |
| `$10` | `\$10` (escape dollar) |

---

## Tips

### Test Your Regex

Use [regex101.com](https://regex101.com) with **ECMAScript/JavaScript** mode.

### Non-greedy Regex

Add `?` after quantifiers to match minimum:
- `.*` → greedy (everything to last match)
- `.*?` → non-greedy (everything to first match)

### Capture Groups

Only the **first group** `()` content is extracted. Use `(?:...)` for non-capturing groups.

```
Regex: price:\s*(\d+)\s*(?:€|EUR)
Captures: only the number
```
