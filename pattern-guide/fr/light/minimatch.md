# Patterns d'URL - Aide-memoire

## Syntaxe

| Pattern | Signification |
|---------|---------------|
| `*` | Caracteres dans UN niveau |
| `**` | Caracteres sur PLUSIEURS niveaux |
| `?` | UN caractere |
| `[abc]` | Un parmi a, b, c |
| `[!abc]` | Sauf a, b, c |

## Options

| Option | Logique | Exemple |
|--------|---------|---------|
| `includePatterns` | Tous doivent matcher (AND) | `["**/blog/**"]` |
| `excludePatterns` | Un suffit pour exclure (OR) | `["**/*.pdf"]` |
| `subdomainsPatterns` | Autorise sous-domaines | `["blog.", "shop."]` |

## Exemples rapides

```json
// Blog uniquement
{ "includePatterns": ["**/blog/**"] }

// Exclure PDFs et pagination
{ "excludePatterns": ["**/*.pdf", "**/page/[2-9]/**"] }

// Sous-domaines de langue
{ "subdomainsPatterns": ["fr.", "en.", "de."] }

// Blog sauf 2020
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

## Erreurs frequentes

| Faux | Juste |
|------|-------|
| `/blog/` | `**/blog/**` |
| `blog` (sous-domaine) | `blog.` |
