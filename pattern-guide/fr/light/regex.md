# Regex - Aide-memoire

## Syntaxe

Pas de prefixe. Pas de `/`. Echapper `\` en `\\` dans JSON.

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

```json
// Prix euros
{ "extractPattern": "(\\d+[,.]\\d{2})\\s*€", "extractName": "prix" }

// Telephone FR
{ "extractPattern": "(0[1-9](?:[\\s.-]?\\d{2}){4})", "extractName": "tel" }

// Email
{ "extractPattern": "([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,})", "extractName": "email" }

// Code postal FR
{ "extractPattern": "\\b(\\d{5})\\b", "extractName": "cp" }

// Date JJ/MM/AAAA
{ "extractPattern": "(\\d{2}/\\d{2}/\\d{4})", "extractName": "date" }

// Schema.org types
{ "extractPattern": "\"@type\"\\s*:\\s*\"([^\"]+)\"", "extractName": "schema" }
```

## Erreurs frequentes

| Faux | Juste |
|------|-------|
| `/\d+/` | `\\d+` |
| `prix: \d+` | `prix: (\\d+)` (capture) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |

## Test

[regex101.com](https://regex101.com) (mode ECMAScript)
