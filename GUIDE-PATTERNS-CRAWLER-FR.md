# Guide des Patterns pour le Crawler

Ce guide explique comment utiliser les patterns (filtres) disponibles dans le crawler pour cibler ou exclure des URLs, et extraire des informations spécifiques des pages.

---

## Table des matieres

1. [Patterns d'inclusion/exclusion d'URLs](#1-patterns-dinclusion-exclusion-durls)
2. [Patterns de sous-domaines](#2-patterns-de-sous-domaines)
3. [Patterns d'extraction (Regex et CSS)](#3-patterns-dextraction-regex-et-css)
4. [Exemples pratiques par cas d'usage](#4-exemples-pratiques-par-cas-dusage)
5. [Erreurs courantes a eviter](#5-erreurs-courantes-a-eviter)

---

## 1. Patterns d'inclusion / exclusion d'URLs

Ces patterns servent a filtrer les URLs que le crawler va explorer. Ils utilisent la syntaxe **glob** (comme les wildcards sur votre ordinateur).

### Comment ca fonctionne

- **includePatterns** : Le crawler ne suit QUE les URLs qui correspondent a TOUS les patterns d'inclusion
- **excludePatterns** : Le crawler ignore les URLs qui correspondent a AU MOINS UN pattern d'exclusion

**Important** : Ces patterns s'appliquent uniquement au **chemin** de l'URL (la partie apres le domaine), pas au domaine lui-meme.

### Syntaxe des patterns glob

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `*` | N'importe quels caracteres dans UN niveau de dossier | `/blog/*` matche `/blog/article` mais PAS `/blog/2024/article` |
| `**` | N'importe quels caracteres sur PLUSIEURS niveaux | `/blog/**` matche `/blog/article` ET `/blog/2024/article` |
| `?` | UN seul caractere | `/page?/` matche `/page1/` ou `/pageA/` |
| `[abc]` | Un caractere parmi ceux listes | `/[abc].html` matche `/a.html`, `/b.html`, `/c.html` |
| `[!abc]` | Un caractere SAUF ceux listes | `/[!0-9]*` exclut les chemins commencant par un chiffre |

### Options du crawler

Le matching est **insensible a la casse** (majuscules = minuscules) et supporte le **matching partiel**.

### Exemples d'inclusion (includePatterns)

**Crawler uniquement le blog :**
```
**/blog/**
```
Resultat : Ne crawle que les URLs contenant `/blog/` dans le chemin.

**Crawler uniquement les pages produits :**
```
**/products/**
```

**Crawler uniquement les pages en francais :**
```
**/fr/**
```

**Crawler uniquement certaines categories :**
```json
["**/category/**", "**/categorie/**"]
```
Note : Avec plusieurs patterns d'inclusion, TOUS doivent matcher (c'est un AND).

### Exemples d'exclusion (excludePatterns)

**Exclure les pages de compte utilisateur :**
```
**/account/**
```

**Exclure les PDFs :**
```
**/*.pdf
```

**Exclure plusieurs types de fichiers :**
```json
["**/*.pdf", "**/*.zip", "**/*.exe"]
```

**Exclure les pages de pagination au-dela de la page 5 :**
```json
["**/page/[6-9]/**", "**/page/[1-9][0-9]/**"]
```

**Exclure les parametres de tri/filtrage :**
```
**?*sort=**
```

**Exclure les sections admin et panier :**
```json
["**/admin/**", "**/cart/**", "**/checkout/**", "**/login/**"]
```

### Combinaison inclusion + exclusion

**Cas pratique** : Je veux crawler le blog SAUF les articles de 2020

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/blog/2020/**"]
}
```

**Cas pratique** : Je veux crawler les produits en francais sauf les produits en rupture

```json
{
  "includePatterns": ["**/fr/produits/**"],
  "excludePatterns": ["**/rupture/**", "**/out-of-stock/**"]
}
```

---

## 2. Patterns de sous-domaines

Ces patterns permettent d'autoriser le crawl de sous-domaines specifiques.

### Comment ca fonctionne

Par defaut, le crawler reste sur le domaine principal. Si votre site a des sous-domaines (blog.example.com, shop.example.com), vous devez les autoriser explicitement.

Le pattern matche la partie AVANT le domaine principal.

### Exemples

**Domaine principal** : `example.com`

**Autoriser le sous-domaine blog :**
```
blog.
```
Resultat : Autorise `blog.example.com`

**Autoriser plusieurs sous-domaines :**
```json
["blog.", "shop.", "help."]
```

**Autoriser tous les sous-domaines commencant par "fr" :**
```
fr*
```
Resultat : Autorise `fr.example.com`, `fr-be.example.com`, `france.example.com`

**Autoriser tous les sous-domaines :**
```
*
```

**Autoriser les sous-domaines de langue :**
```json
["fr.", "en.", "es.", "de."]
```

---

## 3. Patterns d'extraction (Regex et CSS)

Cette fonctionnalite permet d'extraire des informations specifiques de chaque page crawlee.

### Deux modes disponibles

1. **Mode CSS** : Utilise des selecteurs CSS pour cibler des elements HTML
2. **Mode Regex** : Utilise des expressions regulieres pour trouver des patterns dans le code source

### Mode CSS Selector

Prefixez votre pattern avec `css:` pour utiliser ce mode.

**Syntaxe** : `css:<selecteur CSS>`

#### Selecteurs CSS de base

| Selecteur | Description | Exemple |
|-----------|-------------|---------|
| `tag` | Selectionne par nom de balise | `css:h1` |
| `.classe` | Selectionne par classe | `css:.product-price` |
| `#id` | Selectionne par ID | `css:#main-content` |
| `[attr]` | Selectionne par attribut | `css:[data-product-id]` |
| `[attr="val"]` | Attribut avec valeur exacte | `css:[rel="canonical"]` |
| `[attr*="val"]` | Attribut contenant une valeur | `css:[class*="price"]` |
| `parent enfant` | Element enfant | `css:.product .price` |
| `parent > enfant` | Enfant direct | `css:ul > li` |

#### Exemples pratiques CSS

**Extraire tous les prix :**
```
css:.price
```
ou
```
css:[class*="price"]
```

**Extraire les titres de produits :**
```
css:.product-title
```
ou
```
css:h1.product-name
```

**Extraire les SKU/references :**
```
css:[data-sku]
```
ou
```
css:.product-reference
```

**Extraire les notes/avis :**
```
css:[class*="rating"]
```

**Extraire le contenu principal :**
```
css:main
```
ou
```
css:#content
```
ou
```
css:article
```

**Extraire les meta descriptions :**
```
css:meta[name="description"]
```

### Mode Regex (Expressions Regulieres)

Ecrivez directement votre regex (sans le prefixe css:).

**Important** :
- N'encadrez PAS votre regex avec des `/` (le crawler les retire automatiquement)
- Utilisez des **groupes de capture** `()` pour extraire une partie specifique
- Le flag `g` (global) est applique automatiquement

#### Syntaxe Regex de base

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `.` | N'importe quel caractere | `a.c` matche "abc", "a1c" |
| `*` | 0 ou plusieurs fois | `ab*c` matche "ac", "abc", "abbc" |
| `+` | 1 ou plusieurs fois | `ab+c` matche "abc", "abbc" mais PAS "ac" |
| `?` | 0 ou 1 fois | `colou?r` matche "color" et "colour" |
| `\d` | Un chiffre | `\d+` matche "123", "42" |
| `\s` | Un espace/tabulation | `hello\s+world` |
| `[abc]` | Un caractere parmi | `[aeiou]` matche les voyelles |
| `[^abc]` | Un caractere sauf | `[^0-9]` matche tout sauf chiffres |
| `(...)` | Groupe de capture | `prix: (\d+)` capture le nombre |
| `(?:...)` | Groupe sans capture | `(?:www\.)?` |
| `^` | Debut de ligne | `^Titre` |
| `$` | Fin de ligne | `\.html$` |
| `\b` | Limite de mot | `\bprix\b` |

#### Exemples pratiques Regex

**Extraire des prix (format europeen) :**
```
(\d+[,\.]\d{2})\s*EUR
```
Capture : "29,99" de "29,99 EUR"

**Extraire des prix (avec symbole euro) :**
```
(\d+[,\.]\d{2})\s*€
```

**Extraire des numeros de telephone francais :**
```
(0[1-9](?:[\s.-]?\d{2}){4})
```
Capture : "01 23 45 67 89" ou "0123456789"

**Extraire des emails :**
```
([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})
```

**Extraire des codes postaux francais :**
```
\b(\d{5})\b
```

**Extraire des dates (format JJ/MM/AAAA) :**
```
(\d{2}/\d{2}/\d{4})
```

**Extraire des references produit (format ABC-12345) :**
```
([A-Z]{2,4}-\d{4,6})
```

**Extraire le contenu d'une balise meta specifique :**
```
<meta name="author" content="([^"]+)"
```

**Extraire un data-attribute :**
```
data-product-id="(\d+)"
```

**Extraire des URLs d'images :**
```
<img[^>]+src="([^"]+)"
```

### Difference entre CSS et Regex

| Critere | CSS Selector | Regex |
|---------|--------------|-------|
| Facilite | Plus simple | Plus complexe |
| Precision | Element HTML entier | Texte exact |
| Performance | Plus rapide | Plus lent |
| Cas d'usage | Structure HTML connue | Patterns dans le texte |

**Conseil** : Privilegiez les CSS selectors quand possible. Utilisez les Regex pour des extractions complexes ou quand la structure HTML n'est pas fiable.

---

## 4. Exemples pratiques par cas d'usage

### Cas 1 : Audit SEO d'un blog

**Objectif** : Crawler uniquement le blog et extraire les dates de publication

```json
{
  "includePatterns": ["**/blog/**"],
  "excludePatterns": ["**/author/**", "**/tag/**", "**/category/**"],
  "extractPattern": "css:time[datetime]",
  "extractName": "date_publication"
}
```

### Cas 2 : Audit e-commerce

**Objectif** : Crawler les produits et extraire les prix

```json
{
  "includePatterns": ["**/product/**", "**/produit/**"],
  "excludePatterns": ["**/cart/**", "**/checkout/**", "**/account/**"],
  "extractPattern": "css:.product-price",
  "extractName": "prix"
}
```

### Cas 3 : Site multilingue

**Objectif** : Crawler uniquement la version francaise

```json
{
  "includePatterns": ["**/fr/**"],
  "subdomainsPatterns": ["fr."]
}
```

### Cas 4 : Extraire les numeros de telephone

**Objectif** : Trouver tous les numeros de telephone sur le site

```json
{
  "extractPattern": "(0[1-9](?:[\\s.-]?\\d{2}){4})",
  "extractName": "telephones"
}
```

### Cas 5 : Verifier la presence de donnees structurees

**Objectif** : Extraire les types de schema.org presents

```json
{
  "extractPattern": "\"@type\"\\s*:\\s*\"([^\"]+)\"",
  "extractName": "schema_types"
}
```

### Cas 6 : Crawler un site avec sous-domaines regionalises

**Objectif** : Crawler toutes les versions europeennes

```json
{
  "subdomainsPatterns": ["fr.", "de.", "es.", "it.", "uk.", "eu."]
}
```

### Cas 7 : Exclure les pages a faible valeur SEO

```json
{
  "excludePatterns": [
    "**/search/**",
    "**/recherche/**",
    "**?s=**",
    "**/page/[2-9]/**",
    "**/page/[1-9][0-9]/**",
    "**/tag/**",
    "**/tags/**",
    "**/author/**",
    "**/auteur/**",
    "**?orderby=**",
    "**?sort=**",
    "**?filter=**"
  ]
}
```

### Cas 8 : Extraire les meta robots

```json
{
  "extractPattern": "<meta[^>]*name=[\"']robots[\"'][^>]*content=[\"']([^\"']+)[\"']",
  "extractName": "meta_robots"
}
```

---

## 5. Erreurs courantes a eviter

### Patterns d'URL

**ERREUR** : Oublier les `**` pour le matching multi-niveaux
```
/blog/     (ne matche que /blog/)
**/blog/** (matche /fr/blog/, /2024/blog/, etc.)
```

**ERREUR** : Inclure le domaine dans le pattern
```
https://example.com/blog/**  (FAUX)
**/blog/**                   (CORRECT)
```

**ERREUR** : Oublier le slash final pour les dossiers
```
**/blog    (peut ne pas matcher /blog/)
**/blog/** (plus sur)
```

### Sous-domaines

**ERREUR** : Oublier le point final
```
blog   (FAUX - matcherait "blog" n'importe ou)
blog.  (CORRECT - matche "blog." comme prefixe)
```

### CSS Selectors

**ERREUR** : Oublier le prefixe `css:`
```
.product-price       (sera interprete comme regex!)
css:.product-price   (CORRECT)
```

**ERREUR** : Utiliser des selecteurs invalides
```
css:.prix produit    (FAUX - espace sans contexte)
css:.prix.produit    (element avec les 2 classes)
css:.prix .produit   (enfant avec classe .produit)
```

### Regex

**ERREUR** : Ne pas echapper les caracteres speciaux
```
prix: €10.99    (FAUX - . et € sont speciaux)
prix: €10\.99   (CORRECT)
```

**ERREUR** : Encadrer avec des slashes
```
/\d+/           (FAUX - le crawler les retire deja)
\d+             (CORRECT)
```

**ERREUR** : Oublier les groupes de capture
```
prix: \d+       (capture tout "prix: 10")
prix: (\d+)     (capture seulement "10")
```

**ERREUR** : Regex trop gourmande
```
<div>.*</div>   (capture de la premiere <div> a la derniere </div>)
<div>.*?</div>  (CORRECT - capture la plus petite correspondance)
```

---

## Resume rapide

| Option | Format | Exemple |
|--------|--------|---------|
| `includePatterns` | Array de globs | `["**/blog/**"]` |
| `excludePatterns` | Array de globs | `["**/*.pdf", "**/admin/**"]` |
| `subdomainsPatterns` | Array de globs | `["blog.", "shop."]` |
| `extractPattern` | String CSS ou Regex | `"css:.price"` ou `"(\d+,\d{2})€"` |
| `extractName` | String | `"prix_produit"` |

**Besoin d'aide ?** Testez vos regex sur [regex101.com](https://regex101.com) (selectionnez JavaScript/ECMAScript) et vos CSS selectors dans les DevTools de votre navigateur (Console > `document.querySelectorAll('.votre-selecteur')`).
