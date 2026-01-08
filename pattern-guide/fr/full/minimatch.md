# Patterns d'URL (Minimatch)

Ces patterns servent a filtrer les URLs que le crawler va explorer. Ils utilisent la syntaxe **glob** (comme les wildcards).

---

## Comment ca fonctionne

- **includePatterns** : Le crawler ne suit QUE les URLs qui correspondent a TOUS les patterns
- **excludePatterns** : Le crawler ignore les URLs qui correspondent a AU MOINS UN pattern
- **subdomainsPatterns** : Autorise le crawl de sous-domaines specifiques

**Important** : Ces patterns s'appliquent au **chemin** de l'URL (apres le domaine), pas au domaine lui-meme.

---

## Syntaxe des patterns

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `*` | N'importe quels caracteres dans UN niveau | `/blog/*` matche `/blog/article` mais PAS `/blog/2024/article` |
| `**` | N'importe quels caracteres sur PLUSIEURS niveaux | `/blog/**` matche `/blog/article` ET `/blog/2024/article` |
| `?` | UN seul caractere | `/page?/` matche `/page1/` ou `/pageA/` |
| `[abc]` | Un caractere parmi ceux listes | `/[abc].html` matche `/a.html`, `/b.html`, `/c.html` |
| `[!abc]` | Un caractere SAUF ceux listes | `/[!0-9]*` exclut les chemins commencant par un chiffre |

**Options** : Matching insensible a la casse et partiel active.

---

## Patterns d'inclusion

Avec plusieurs patterns d'inclusion, **TOUS doivent matcher** (AND).

| Objectif | Pattern |
|----------|---------|
| Tout le blog | `**/blog/**` |
| Pages produits | `**/product/**` |
| Version francaise | `**/fr/**` |
| Categorie specifique | `**/category/chaussures/**` |

---

## Patterns d'exclusion

Avec plusieurs patterns d'exclusion, **UN SEUL suffit** pour exclure (OR).

| Objectif | Pattern |
|----------|---------|
| Exclure PDFs | `**/*.pdf` |
| Exclure pagination | `**/page/[2-9]/**`, `**/page/[1-9][0-9]/**` |
| Exclure recherche | `**/search/**`, `**?s=**` |
| Exclure admin | `**/admin/**`, `**/wp-admin/**` |
| Exclure panier | `**/cart/**`, `**/checkout/**` |
| Exclure compte | `**/account/**`, `**/my-account/**` |
| Exclure tri/filtres | `**?*sort=**`, `**?*orderby=**` |

---

## Patterns de sous-domaines

Par defaut, le crawler reste sur le domaine principal. Le pattern matche la partie **AVANT** le domaine.

| Objectif | Pattern |
|----------|---------|
| Sous-domaine blog | `blog.` |
| Sous-domaine shop | `shop.` |
| Langues EU | `["fr.", "en.", "de.", "es.", "it."]` |
| Tous sous-domaines | `*` |

**Attention** : N'oubliez pas le point final (`blog.` et non `blog`).

---

## Cas d'application

### 1. Audit SEO d'un blog

Crawler uniquement le blog en excluant les pages a faible valeur.

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

### 2. Audit e-commerce

Crawler les fiches produits en excluant le tunnel d'achat.

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

### 3. Site multilingue - Version francaise uniquement

```json
{
  "includePatterns": ["**/fr/**"]
}
```

Ou si le site utilise des sous-domaines :

```json
{
  "subdomainsPatterns": ["fr."]
}
```

### 4. Site multilingue - Toutes les versions EU

```json
{
  "subdomainsPatterns": ["fr.", "de.", "es.", "it.", "uk.", "nl.", "be."]
}
```

### 5. Exclure toutes les pages a faible valeur SEO

```json
{
  "excludePatterns": [
    "**/search/**",
    "**/recherche/**",
    "**?s=**",
    "**/tag/**",
    "**/tags/**",
    "**/author/**",
    "**/auteur/**",
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

### 6. Crawler le blog SAUF une annee specifique

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

### 7. Crawler uniquement les pages de categorie

```json
{
  "includePatterns": ["**/category/**"],
  "excludePatterns": ["**/page/[2-9]/**"]
}
```

---

## Erreurs courantes

| Erreur | Correction |
|--------|------------|
| `/blog/` | `**/blog/**` (oubli des `**`) |
| `https://site.com/blog/**` | `**/blog/**` (pas de domaine) |
| `**/blog` | `**/blog/**` (oubli du slash final) |
| `blog` (sous-domaine) | `blog.` (oubli du point) |

---

## Test et validation

Pour tester vos patterns, vous pouvez utiliser :
- [Globster.xyz](https://globster.xyz/) pour tester les patterns glob
- La console du navigateur avec des exemples d'URLs
