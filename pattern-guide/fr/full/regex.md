# Expressions Regulieres (Regex)

Les regex permettent d'extraire du texte en matchant des patterns dans le code source HTML.

---

## Comment ca fonctionne

Ecrivez directement votre regex dans le champ **Extraction Pattern** (sans prefixe).

**Important** :
- N'encadrez PAS avec des `/`
- Utilisez des **groupes de capture** `()` pour extraire une partie specifique
- Le flag `g` (global) est applique automatiquement

---

## Syntaxe de base

### Caracteres speciaux

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `.` | N'importe quel caractere | `a.c` → "abc", "a1c" |
| `\d` | Un chiffre (0-9) | `\d+` → "123" |
| `\D` | Pas un chiffre | `\D+` → "abc" |
| `\w` | Lettre, chiffre ou _ | `\w+` → "hello_123" |
| `\W` | Pas un mot | `\W+` → "!@#" |
| `\s` | Espace, tab, newline | `hello\s+world` |
| `\S` | Pas un espace | `\S+` → "hello" |

### Quantificateurs

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `*` | 0 ou plusieurs fois | `ab*c` → "ac", "abc", "abbc" |
| `+` | 1 ou plusieurs fois | `ab+c` → "abc", "abbc" (pas "ac") |
| `?` | 0 ou 1 fois | `colou?r` → "color", "colour" |
| `{n}` | Exactement n fois | `\d{5}` → "75001" |
| `{n,}` | Au moins n fois | `\d{2,}` → "12", "123", "1234" |
| `{n,m}` | Entre n et m fois | `\d{2,4}` → "12", "123", "1234" |

### Classes de caracteres

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `[abc]` | Un caractere parmi | `[aeiou]` → voyelles |
| `[^abc]` | Un caractere sauf | `[^0-9]` → pas un chiffre |
| `[a-z]` | Plage de caracteres | `[a-zA-Z]` → lettres |
| `[0-9]` | Equivalent a `\d` | `[0-9]+` → nombres |

### Ancres et limites

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `^` | Debut de ligne | `^Titre` |
| `$` | Fin de ligne | `\.html$` |
| `\b` | Limite de mot | `\bprix\b` → "prix" seul |

### Groupes

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `(...)` | Groupe de capture | `prix: (\d+)` → capture le nombre |
| `(?:...)` | Groupe sans capture | `(?:www\.)?` |
| `\|` | Alternative (OU) | `(chat\|chien)` |

---

## Cas d'application

### 1. Extraire des prix

| Methode | Extraction Pattern |
|---------|-------------------|
| Format europeen (29,99 €) | `(\d+[,.]\d{2})\s*€` |
| Format US ($29.99) | `\$(\d+\.\d{2})` |
| Prix avec EUR | `(\d+[,.]\d{2})\s*EUR` |

### 2. Extraire des numeros de telephone

| Methode | Extraction Pattern |
|---------|-------------------|
| Format francais | `(0[1-9](?:[\s.-]?\d{2}){4})` |
| Format international | `(\+33[\s.-]?[1-9](?:[\s.-]?\d{2}){4})` |

Matche : "01 23 45 67 89", "0123456789", "01.23.45.67.89"

### 3. Extraire des emails

| Methode | Extraction Pattern |
|---------|-------------------|
| Email standard | `([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` |

### 4. Extraire des codes postaux

| Methode | Extraction Pattern |
|---------|-------------------|
| France (5 chiffres) | `\b(\d{5})\b` |
| UK | `([A-Z]{1,2}\d[A-Z\d]?\s?\d[A-Z]{2})` |

### 5. Extraire des dates

| Methode | Extraction Pattern |
|---------|-------------------|
| Format JJ/MM/AAAA | `(\d{2}/\d{2}/\d{4})` |
| Format AAAA-MM-JJ | `(\d{4}-\d{2}-\d{2})` |
| Format textuel | `(\d{1,2}\s+(?:janvier\|fevrier\|mars\|avril\|mai\|juin\|juillet\|aout\|septembre\|octobre\|novembre\|decembre)\s+\d{4})` |

### 6. Extraire des references produit

| Methode | Extraction Pattern |
|---------|-------------------|
| Format ABC-12345 | `([A-Z]{2,4}-\d{4,6})` |
| Format EAN/GTIN | `\b(\d{13})\b` |

### 7. Extraire des donnees structurees

| Methode | Extraction Pattern |
|---------|-------------------|
| Types schema.org | `"@type"\s*:\s*"([^"]+)"` |

### 8. Extraire des meta tags specifiques

| Methode | Extraction Pattern |
|---------|-------------------|
| Meta author | `<meta[^>]*name=["']author["'][^>]*content=["']([^"']+)["']` |
| Meta robots | `<meta[^>]*name=["']robots["'][^>]*content=["']([^"']+)["']` |

### 9. Extraire des data-attributes

| Methode | Extraction Pattern |
|---------|-------------------|
| data-product-id | `data-product-id=["'](\d+)["']` |
| data-price | `data-price=["']([\d.,]+)["']` |

### 10. Extraire des URLs

| Methode | Extraction Pattern |
|---------|-------------------|
| URLs d'images | `<img[^>]+src=["']([^"']+)["']` |
| URLs de liens | `<a[^>]+href=["']([^"']+)["']` |

---

## Erreurs courantes

| Erreur | Correction |
|--------|------------|
| `/\d+/` | `\d+` (pas de slashes) |
| `prix: \d+` | `prix: (\d+)` (groupe de capture) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |
| `10.99` | `10\.99` (echapper le point) |
| `$10` | `\$10` (echapper le dollar) |

---

## Astuces

### Tester vos regex

Utilisez [regex101.com](https://regex101.com) avec le mode **ECMAScript/JavaScript**.

### Regex non-greedy

Ajoutez `?` apres les quantificateurs pour matcher le minimum :
- `.*` → greedy (tout jusqu'au dernier match)
- `.*?` → non-greedy (tout jusqu'au premier match)

### Groupes de capture

Seul le contenu du **premier groupe** `()` est extrait. Si vous avez plusieurs groupes, utilisez `(?:...)` pour les groupes non-capturants.

```
Regex: prix:\s*(\d+)\s*(?:€|EUR)
Capture: uniquement le nombre
```
