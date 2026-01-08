# URL Patterns (Minimatch)

These patterns filter which URLs the crawler will explore. They use **glob** syntax (like wildcards).

---

## How It Works

In the crawl configuration form, you have 3 URL filtering fields:

| Field | Behavior |
|-------|----------|
| **Include Patterns** | The crawler ONLY follows URLs that match ALL patterns |
| **Exclude Patterns** | The crawler ignores URLs that match AT LEAST ONE pattern |
| **Subdomains Patterns** | Allows crawling specific subdomains |

**Important**: These patterns apply to the URL **path** (after the domain), not the domain itself.

Each field allows multiple patterns (+ button). Enter one pattern per line.

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

| Goal | Pattern to enter |
|------|------------------|
| Blog only | `**/blog/**` |
| Product pages | `**/product/**` |
| English version | `**/en/**` |
| Specific category | `**/category/shoes/**` |

---

## Exclude Patterns

With multiple exclude patterns, **ONE is enough** to exclude (OR).

| Goal | Pattern to enter |
|------|------------------|
| Exclude PDFs | `**/*.pdf` |
| Exclude pagination | `**/page/[2-9]/**` |
| Exclude pagination 10+ | `**/page/[1-9][0-9]/**` |
| Exclude search | `**/search/**` |
| Exclude search (param) | `**?s=**` |
| Exclude admin | `**/admin/**` |
| Exclude wp-admin | `**/wp-admin/**` |
| Exclude cart | `**/cart/**` |
| Exclude checkout | `**/checkout/**` |
| Exclude account | `**/account/**` |
| Exclude sorting | `**?*sort=**` |
| Exclude filters | `**?*orderby=**` |

---

## Subdomain Patterns

By default, the crawler stays on the main domain. The pattern matches the part **BEFORE** the domain.

| Goal | Pattern to enter |
|------|------------------|
| Blog subdomain | `blog.` |
| Shop subdomain | `shop.` |
| All subdomains | `*` |

**Note**: Don't forget the trailing dot (`blog.` not `blog`).

For EU languages, add one pattern per subdomain:
- `en.`
- `fr.`
- `de.`
- `es.`
- `it.`

---

## Understanding Include / Exclude Logic

**Important**: If you define an Include Pattern, only URLs that match will be crawled. Everything else is **automatically excluded**.

Exclude Patterns are only useful to:
1. **Refine** what's already included (e.g., blog except pagination)
2. **Filter a full crawl** when you have no Include Pattern

---

## Use Cases

### 1. Blog SEO Audit

**Include Patterns**:
| Pattern |
|---------|
| `**/blog/**` |

That's it! Pages like `/cart/`, `/admin/`, etc. don't match `**/blog/**` so they're already excluded.

To refine (optional) - exclude pagination and author pages from the blog:

**Exclude Patterns**:
| Pattern |
|---------|
| `**/page/[2-9]/**` |
| `**/page/[1-9][0-9]/**` |
| `**/author/**` |

---

### 2. E-commerce Audit - Product Pages Only

**Include Patterns**:
| Pattern |
|---------|
| `**/product/**` |

To refine (optional) - exclude filter variations:

**Exclude Patterns**:
| Pattern |
|---------|
| `**?*sort=**` |
| `**?*filter=**` |

---

### 3. Multilingual Site - English Only

**Include Patterns**:
| Pattern |
|---------|
| `**/en/**` |

Or if using subdomains:

**Subdomains Patterns**:
| Pattern |
|---------|
| `en.` |

---

### 4. Multilingual Site - All EU Versions

**Subdomains Patterns**:
| Pattern |
|---------|
| `en.` |
| `fr.` |
| `de.` |
| `es.` |
| `it.` |
| `nl.` |
| `be.` |

---

### 5. Full Crawl EXCEPT Low SEO Value Pages

When you want to crawl the entire site but exclude certain sections:

**Exclude Patterns**:
| Pattern |
|---------|
| `**/search/**` |
| `**?s=**` |
| `**/tag/**` |
| `**/author/**` |
| `**/page/[2-9]/**` |
| `**/page/[1-9][0-9]/**` |
| `**/admin/**` |
| `**/wp-admin/**` |
| `**/cart/**` |
| `**/checkout/**` |
| `**/account/**` |
| `**/login/**` |
| `**/*.pdf` |

---

### 6. Crawl Blog Except Specific Year

**Include Patterns**:
| Pattern |
|---------|
| `**/blog/**` |

**Exclude Patterns**:
| Pattern |
|---------|
| `**/blog/2020/**` |

---

### 7. Crawl Category Pages Only (No Pagination)

**Include Patterns**:
| Pattern |
|---------|
| `**/category/**` |

**Exclude Patterns**:
| Pattern |
|---------|
| `**/page/[2-9]/**` |

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `/blog/` | `**/blog/**` (missing `**`) |
| `https://site.com/blog/**` | `**/blog/**` (no domain) |
| `**/blog` | `**/blog/**` (missing trailing slash) |
| `blog` (subdomain) | `blog.` (missing dot) |

