# URL Patterns - Cheat Sheet

## Syntax

| Pattern | Meaning |
|---------|---------|
| `*` | Characters in ONE level |
| `**` | Characters across MULTIPLE levels |
| `?` | ONE character |
| `[abc]` | One of a, b, c |
| `[!abc]` | Not a, b, c |

## Form Fields

| Field | Logic |
|-------|-------|
| Include Patterns | All must match (AND) |
| Exclude Patterns | One excludes (OR) |
| Subdomains Patterns | Allow subdomains |

## Quick Examples

**Blog only** - Include Patterns:
| Pattern |
|---------|
| `**/blog/**` |

**Exclude PDFs and pagination** - Exclude Patterns:
| Pattern |
|---------|
| `**/*.pdf` |
| `**/page/[2-9]/**` |

**Language subdomains** - Subdomains Patterns:
| Pattern |
|---------|
| `en.` |
| `fr.` |
| `de.` |

**Blog except 2020**:

Include Patterns:
| Pattern |
|---------|
| `**/blog/**` |

Exclude Patterns:
| Pattern |
|---------|
| `**/blog/2020/**` |

## Common Mistakes

| Wrong | Right |
|-------|-------|
| `/blog/` | `**/blog/**` |
| `blog` (subdomain) | `blog.` |
