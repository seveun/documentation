# Regex - Aide-memoire

## Syntaxe

Pas de prefixe. Pas de `/`.

| Pattern | Signification |
|---------|---------------|
| `.` | N'importe quel caractere |
| `\d` | Chiffre |
| `\s` | Espace |
| `\w` | Lettre/chiffre/_ |
| `+` | 1 ou plus |
| `*` | 0 ou plus |
| `?` | 0 ou 1 |
| `{n}` | Exactement n fois |
| `[abc]` | Un parmi |
| `[^abc]` | Sauf |
| `(...)` | Groupe de capture |
| `\b` | Limite de mot |

## Exemples rapides

| Objectif | Extraction Pattern |
|----------|-------------------|
| Prix euros | `(\d+[,.]\d{2})\s*€` |
| Telephone FR | `(0[1-9](?:[\s.-]?\d{2}){4})` |
| Email | `([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` |
| Code postal FR | `\b(\d{5})\b` |
| Date JJ/MM/AAAA | `(\d{2}/\d{2}/\d{4})` |
| Schema.org types | `"@type"\s*:\s*"([^"]+)"` |

## Erreurs frequentes

| Faux | Juste |
|------|-------|
| `/\d+/` | `\d+` |
| `prix: \d+` | `prix: (\d+)` (capture) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |

## Test

[regex101.com](https://regex101.com) (mode ECMAScript)
