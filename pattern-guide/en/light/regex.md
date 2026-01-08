# Regex - Cheat Sheet

## Syntax

No prefix. No `/`.

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

| Purpose | Extraction Pattern |
|---------|-------------------|
| US price | `\$(\d+\.\d{2})` |
| US phone | `(\d{3}[-.\\s]?\d{3}[-.\\s]?\d{4})` |
| Email | `([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` |
| US zip code | `\b(\d{5})\b` |
| Date MM/DD/YYYY | `(\d{2}/\d{2}/\d{4})` |
| Schema.org types | `"@type"\s*:\s*"([^"]+)"` |

## Common Mistakes

| Wrong | Right |
|-------|-------|
| `/\d+/` | `\d+` |
| `price: \d+` | `price: (\d+)` (capture) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |

## Testing

[regex101.com](https://regex101.com) (ECMAScript mode)
