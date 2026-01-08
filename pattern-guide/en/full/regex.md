# Regular Expressions (Regex)

Regex extracts text by matching patterns in the HTML source code.

---

## How It Works

Write your regex directly in `extractPattern` (no prefix).

```json
{
  "extractPattern": "(\\d+[,.]\\d{2})\\s*\\$",
  "extractName": "price"
}
```

**Important**:
- Do NOT wrap with `/`
- Use **capture groups** `()` to extract specific parts
- The `g` flag (global) is applied automatically
- Escape backslashes: `\d` becomes `\\d` in JSON

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

**US format ($29.99):**
```json
{
  "extractPattern": "\\$(\\d+\\.\\d{2})",
  "extractName": "price"
}
```

**European format (29,99 €):**
```json
{
  "extractPattern": "(\\d+[,.]\\d{2})\\s*€",
  "extractName": "price"
}
```

**With EUR:**
```json
{
  "extractPattern": "(\\d+[,.]\\d{2})\\s*EUR",
  "extractName": "price"
}
```

### 2. Extract Phone Numbers

**US format:**
```json
{
  "extractPattern": "(\\d{3}[-.\\s]?\\d{3}[-.\\s]?\\d{4})",
  "extractName": "phone"
}
```
Matches: "555-123-4567", "5551234567", "555.123.4567"

**UK format:**
```json
{
  "extractPattern": "(0\\d{2,4}[\\s.-]?\\d{3,4}[\\s.-]?\\d{3,4})",
  "extractName": "phone"
}
```

**International:**
```json
{
  "extractPattern": "(\\+1[\\s.-]?\\d{3}[\\s.-]?\\d{3}[\\s.-]?\\d{4})",
  "extractName": "phone"
}
```

### 3. Extract Emails

```json
{
  "extractPattern": "([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,})",
  "extractName": "email"
}
```

### 4. Extract Zip Codes

**US (5 digits or 5+4):**
```json
{
  "extractPattern": "\\b(\\d{5}(?:-\\d{4})?)\\b",
  "extractName": "zip_code"
}
```

**UK:**
```json
{
  "extractPattern": "([A-Z]{1,2}\\d[A-Z\\d]?\\s?\\d[A-Z]{2})",
  "extractName": "postcode"
}
```

### 5. Extract Dates

**MM/DD/YYYY format:**
```json
{
  "extractPattern": "(\\d{2}/\\d{2}/\\d{4})",
  "extractName": "date"
}
```

**YYYY-MM-DD format:**
```json
{
  "extractPattern": "(\\d{4}-\\d{2}-\\d{2})",
  "extractName": "date"
}
```

**Text format:**
```json
{
  "extractPattern": "((?:January|February|March|April|May|June|July|August|September|October|November|December)\\s+\\d{1,2},?\\s+\\d{4})",
  "extractName": "date"
}
```

### 6. Extract Product References

**Format ABC-12345:**
```json
{
  "extractPattern": "([A-Z]{2,4}-\\d{4,6})",
  "extractName": "reference"
}
```

**EAN/GTIN format:**
```json
{
  "extractPattern": "\\b(\\d{13})\\b",
  "extractName": "ean"
}
```

### 7. Extract Structured Data

**Schema.org types:**
```json
{
  "extractPattern": "\"@type\"\\s*:\\s*\"([^\"]+)\"",
  "extractName": "schema_types"
}
```

### 8. Extract Specific Meta Tags

**Meta author content:**
```json
{
  "extractPattern": "<meta[^>]*name=[\"']author[\"'][^>]*content=[\"']([^\"']+)[\"']",
  "extractName": "author"
}
```

**Meta robots content:**
```json
{
  "extractPattern": "<meta[^>]*name=[\"']robots[\"'][^>]*content=[\"']([^\"']+)[\"']",
  "extractName": "meta_robots"
}
```

### 9. Extract Data Attributes

**data-product-id:**
```json
{
  "extractPattern": "data-product-id=[\"'](\\d+)[\"']",
  "extractName": "product_id"
}
```

**data-price:**
```json
{
  "extractPattern": "data-price=[\"']([\\d.,]+)[\"']",
  "extractName": "price"
}
```

### 10. Extract URLs

**Image URLs:**
```json
{
  "extractPattern": "<img[^>]+src=[\"']([^\"']+)[\"']",
  "extractName": "images"
}
```

**Link URLs:**
```json
{
  "extractPattern": "<a[^>]+href=[\"']([^\"']+)[\"']",
  "extractName": "links"
}
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `/\d+/` | `\d+` (no slashes) |
| `\d+` in JSON | `\\d+` (escape backslashes) |
| `price: \d+` | `price: (\d+)` (capture group) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |
| `10.99` | `10\\.99` (escape dot) |
| `$10` | `\\$10` (escape dollar) |

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

### JSON Escaping

In JSON, double the backslashes:
```
Normal regex: \d+
In JSON:      \\d+
```
