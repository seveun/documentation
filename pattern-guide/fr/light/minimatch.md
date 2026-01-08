# Patterns d'URL - Aide-memoire

## Syntaxe

| Pattern | Signification |
|---------|---------------|
| `*` | Caracteres dans UN niveau |
| `**` | Caracteres sur PLUSIEURS niveaux |
| `?` | UN caractere |
| `[abc]` | Un parmi a, b, c |
| `[!abc]` | Sauf a, b, c |

## Champs du formulaire

| Champ | Logique |
|-------|---------|
| Include Patterns | Tous doivent matcher (AND) |
| Exclude Patterns | Un suffit pour exclure (OR) |
| Subdomains Patterns | Autorise sous-domaines |

## Exemples rapides

**Blog uniquement** - Include Patterns :
| Pattern |
|---------|
| `**/blog/**` |

**Exclure PDFs et pagination** - Exclude Patterns :
| Pattern |
|---------|
| `**/*.pdf` |
| `**/page/[2-9]/**` |

**Sous-domaines de langue** - Subdomains Patterns :
| Pattern |
|---------|
| `fr.` |
| `en.` |
| `de.` |

**Blog sauf 2020** :

Include Patterns :
| Pattern |
|---------|
| `**/blog/**` |

Exclude Patterns :
| Pattern |
|---------|
| `**/blog/2020/**` |

## Erreurs frequentes

| Faux | Juste |
|------|-------|
| `/blog/` | `**/blog/**` |
| `blog` (sous-domaine) | `blog.` |
