# URL Patterns - Cheat Sheet

## Syntax

| Pattern | Meaning |
|---------|---------|
| `*` | Characters in ONE level |
| `**` | Characters across MULTIPLE levels |
| `?` | ONE character |
| `[abc]` | One of a, b, c |
| `[!abc]` | Not a, b, c |

## Options

| Option | Logic | Example |
|--------|-------|---------|
| `includePatterns` | All must match (AND) | `["**/blog/**"]` |
| `excludePatterns` | One excludes (OR) | `["**/*.pdf"]` |
| `subdomainsPatterns` | Allow subdomains | `["blog.", "shop."]` |

## Quick Examples

```json
// Blog only
{ "includePatterns": ["**/blog/**"] }

// Exclude PDFs and pagination
{ "excludePatterns": ["**/*.pdf", "**/page/[2-9]/**"] }

// Language subdomains
{ "subdomainsPatterns": ["en.", "fr.", "de."] }

// Blog except 2020
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

## Common Mistakes

| Wrong | Right |
|-------|-------|
| `/blog/` | `**/blog/**` |
| `blog` (subdomain) | `blog.` |
