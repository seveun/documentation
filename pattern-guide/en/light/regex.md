# Regex - Cheat Sheet

## Syntax

No prefix. No `/`. Escape `\` as `\\` in JSON.

| Pattern | Meaning |
|---------|---------|
| `.` | Any character |
| `\d` | Digit |
| `\s` | Space |
| `\w` | Letter/digit/_ |
| `+` | 1 or more |
| `*` | 0 or more |
| `?` | 0 or 1 |
| `{n}` | Exactly n times |
| `[abc]` | One of |
| `[^abc]` | Not |
| `(...)` | Capture group |
| `\b` | Word boundary |

## Quick Examples

```json
// US price
{ "extractPattern": "\\$(\\d+\\.\\d{2})", "extractName": "price" }

// US phone
{ "extractPattern": "(\\d{3}[-.\\s]?\\d{3}[-.\\s]?\\d{4})", "extractName": "phone" }

// Email
{ "extractPattern": "([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,})", "extractName": "email" }

// US zip code
{ "extractPattern": "\\b(\\d{5})\\b", "extractName": "zip" }

// Date MM/DD/YYYY
{ "extractPattern": "(\\d{2}/\\d{2}/\\d{4})", "extractName": "date" }

// Schema.org types
{ "extractPattern": "\"@type\"\\s*:\\s*\"([^\"]+)\"", "extractName": "schema" }
```

## Common Mistakes

| Wrong | Right |
|-------|-------|
| `/\d+/` | `\\d+` |
| `price: \d+` | `price: (\\d+)` (capture) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |

## Testing

[regex101.com](https://regex101.com) (ECMAScript mode)
