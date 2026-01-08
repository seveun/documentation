# URL Patterns (Minimatch)

These patterns filter which URLs the crawler will explore. They use **glob** syntax (like wildcards).

---

## How It Works

- **includePatterns**: The crawler ONLY follows URLs that match ALL patterns
- **excludePatterns**: The crawler ignores URLs that match AT LEAST ONE pattern
- **subdomainsPatterns**: Allows crawling specific subdomains

**Important**: These patterns apply to the URL **path** (after the domain), not the domain itself.

---

## Pattern Syntax

| Pattern | Meaning | Example |
|---------|---------|---------|
| `*` | Any characters within ONE level | `/blog/*` matches `/blog/article` but NOT `/blog/2024/article` |
| `**` | Any characters across MULTIPLE levels | `/blog/**` matches `/blog/article` AND `/blog/2024/article` |
| `?` | ONE single character | `/page?/` matches `/page1/` or `/pageA/` |
| `[abc]` | One character from the list | `/[abc].html` matches `/a.html`, `/b.html`, `/c.html` |
| `[!abc]` | One character NOT in the list | `/[!0-9]*` excludes paths starting with a digit |

**Options**: Case-insensitive and partial matching enabled.

---

## Include Patterns

With multiple include patterns, **ALL must match** (AND).

| Goal | Pattern |
|------|---------|
| Blog only | `**/blog/**` |
| Product pages | `**/product/**` |
| English version | `**/en/**` |
| Specific category | `**/category/shoes/**` |

---

## Exclude Patterns

With multiple exclude patterns, **ONE is enough** to exclude (OR).

| Goal | Pattern |
|------|---------|
| Exclude PDFs | `**/*.pdf` |
| Exclude pagination | `**/page/[2-9]/**`, `**/page/[1-9][0-9]/**` |
| Exclude search | `**/search/**`, `**?s=**` |
| Exclude admin | `**/admin/**`, `**/wp-admin/**` |
| Exclude cart | `**/cart/**`, `**/checkout/**` |
| Exclude account | `**/account/**`, `**/my-account/**` |
| Exclude sorting | `**?*sort=**`, `**?*orderby=**` |

---

## Subdomain Patterns

By default, the crawler stays on the main domain. The pattern matches the part **BEFORE** the domain.

| Goal | Pattern |
|------|---------|
| Blog subdomain | `blog.` |
| Shop subdomain | `shop.` |
| EU languages | `["en.", "fr.", "de.", "es.", "it."]` |
| All subdomains | `*` |

**Note**: Don't forget the trailing dot (`blog.` not `blog`).

---

## Use Cases

### 1. Blog SEO Audit

Crawl only the blog excluding low-value pages.

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": [
    "**/author/**",
    "**/tag/**",
    "**/category/**",
    "**/page/[2-9]/**",
    "**/page/[1-9][0-9]/**"
  ]
}
```

### 2. E-commerce Audit

Crawl product pages excluding checkout funnel.

```json
{
  "includePatterns": ["**/product/**"],
  "excludePatterns": [
    "**/cart/**",
    "**/checkout/**",
    "**/account/**",
    "**/wishlist/**",
    "**?*sort=**",
    "**?*filter=**"
  ]
}
```

### 3. Multilingual Site - English Only

```json
{
  "includePatterns": ["**/en/**"]
}
```

Or if using subdomains:

```json
{
  "subdomainsPatterns": ["en."]
}
```

### 4. Multilingual Site - All EU Versions

```json
{
  "subdomainsPatterns": ["en.", "fr.", "de.", "es.", "it.", "nl.", "be."]
}
```

### 5. Exclude All Low SEO Value Pages

```json
{
  "excludePatterns": [
    "**/search/**",
    "**?s=**",
    "**/tag/**",
    "**/tags/**",
    "**/author/**",
    "**/page/[2-9]/**",
    "**/page/[1-9][0-9]/**",
    "**/admin/**",
    "**/wp-admin/**",
    "**/cart/**",
    "**/checkout/**",
    "**/account/**",
    "**/login/**",
    "**/*.pdf",
    "**/*.zip"
  ]
}
```

### 6. Crawl Blog Except Specific Year

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

### 7. Crawl Category Pages Only

```json
{
  "includePatterns": ["**/category/**"],
  "excludePatterns": ["**/page/[2-9]/**"]
}
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `/blog/` | `**/blog/**` (missing `**`) |
| `https://site.com/blog/**` | `**/blog/**` (no domain) |
| `**/blog` | `**/blog/**` (missing trailing slash) |
| `blog` (subdomain) | `blog.` (missing dot) |

---

## Testing

Test your patterns before entering them in the frontend:
- [regex101.com](https://regex101.com/) - Online pattern tester
