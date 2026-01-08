# Selecteurs CSS

Les selecteurs CSS permettent d'extraire des elements HTML specifiques de chaque page crawlee.

---

## Comment ca fonctionne

Entrez votre selecteur CSS dans le champ d'extraction.

```
extractPattern: ".product-price"
extractName: "prix"
```

Le crawler extrait le **texte** de tous les elements qui matchent le selecteur.

---

## Syntaxe des selecteurs

### Selecteurs de base

| Selecteur | Description | Exemple |
|-----------|-------------|---------|
| `tag` | Par nom de balise | `h1` |
| `.classe` | Par classe | `.product-price` |
| `#id` | Par ID | `#main-content` |
| `tag.classe` | Balise avec classe | `h1.title` |
| `.classe1.classe2` | Element avec 2 classes | `.btn.primary` |

### Selecteurs d'attributs

| Selecteur | Description | Exemple |
|-----------|-------------|---------|
| `[attr]` | A l'attribut | `[data-price]` |
| `[attr="val"]` | Attribut = valeur exacte | `[rel="canonical"]` |
| `[attr*="val"]` | Attribut contient | `[class*="price"]` |
| `[attr^="val"]` | Attribut commence par | `[href^="https"]` |
| `[attr$="val"]` | Attribut finit par | `[src$=".jpg"]` |

### Selecteurs hierarchiques

| Selecteur | Description | Exemple |
|-----------|-------------|---------|
| `parent enfant` | Descendant (n'importe quel niveau) | `.product .price` |
| `parent > enfant` | Enfant direct | `ul > li` |
| `element + suivant` | Frere adjacent | `h1 + p` |
| `element ~ suivants` | Tous les freres suivants | `h1 ~ p` |

### Pseudo-selecteurs utiles

| Selecteur | Description | Exemple |
|-----------|-------------|---------|
| `:first-child` | Premier enfant | `li:first-child` |
| `:last-child` | Dernier enfant | `li:last-child` |
| `:nth-child(n)` | N-ieme enfant | `li:nth-child(2)` |
| `:not(sel)` | Negation | `a:not(.external)` |

---

## Cas d'application

### 1. Extraire les prix

**Par classe :**
```json
{
  "extractPattern": ".price",
  "extractName": "prix"
}
```

**Par classe partielle :**
```json
{
  "extractPattern": "[class*=\"price\"]",
  "extractName": "prix"
}
```

**Prix dans un conteneur produit :**
```json
{
  "extractPattern": ".product .price",
  "extractName": "prix"
}
```

### 2. Extraire les titres de produits

```json
{
  "extractPattern": "h1.product-title",
  "extractName": "titre_produit"
}
```

Ou plus generique :
```json
{
  "extractPattern": "[class*=\"product-title\"]",
  "extractName": "titre_produit"
}
```

### 3. Extraire les references/SKU

**Via data-attribute :**
```json
{
  "extractPattern": "[data-sku]",
  "extractName": "sku"
}
```

**Via classe :**
```json
{
  "extractPattern": ".product-reference",
  "extractName": "reference"
}
```

### 4. Extraire les notes/avis

```json
{
  "extractPattern": "[class*=\"rating\"]",
  "extractName": "note"
}
```

Ou :
```json
{
  "extractPattern": ".review-score",
  "extractName": "note"
}
```

### 5. Extraire le contenu principal

**Via balise semantique :**
```json
{
  "extractPattern": "main",
  "extractName": "contenu"
}
```

**Via ID :**
```json
{
  "extractPattern": "#content",
  "extractName": "contenu"
}
```

**Via balise article :**
```json
{
  "extractPattern": "article",
  "extractName": "contenu"
}
```

### 6. Extraire les dates de publication

**Via balise time :**
```json
{
  "extractPattern": "time[datetime]",
  "extractName": "date_publication"
}
```

**Via classe :**
```json
{
  "extractPattern": ".post-date",
  "extractName": "date_publication"
}
```

### 7. Extraire les categories

```json
{
  "extractPattern": ".breadcrumb a",
  "extractName": "categories"
}
```

Ou :
```json
{
  "extractPattern": "[class*=\"category\"] a",
  "extractName": "categories"
}
```

### 8. Extraire les auteurs

```json
{
  "extractPattern": ".author-name",
  "extractName": "auteur"
}
```

Ou :
```json
{
  "extractPattern": "[rel=\"author\"]",
  "extractName": "auteur"
}
```

### 9. Extraire la disponibilite produit

```json
{
  "extractPattern": "[class*=\"availability\"]",
  "extractName": "disponibilite"
}
```

Ou :
```json
{
  "extractPattern": "[class*=\"stock\"]",
  "extractName": "stock"
}
```

### 10. Extraire les meta tags

**Meta description :**
```json
{
  "extractPattern": "meta[name=\"description\"]",
  "extractName": "meta_description"
}
```

**Meta robots :**
```json
{
  "extractPattern": "meta[name=\"robots\"]",
  "extractName": "meta_robots"
}
```

---

## Erreurs courantes

| Erreur | Correction |
|--------|------------|
| `.price product` | `.price.product` ou `.price .product` |
| `div[class=price]` | `div[class*="price"]` (classe exacte vs contient) |

---

## Astuces

### Tester vos selecteurs

Dans la console du navigateur (F12) :
```javascript
document.querySelectorAll('.votre-selecteur')
```

### Selecteurs robustes

Preferez les selecteurs qui ne changeront pas :
- `[data-*]` attributs custom
- Balises semantiques (`main`, `article`, `nav`)
- Classes metier (`product-price` vs `text-red-500`)

### Quand utiliser CSS vs Regex

| Situation | Choix |
|-----------|-------|
| Element HTML visible | CSS |
| Structure HTML connue | CSS |
| Texte dans le code source | Regex |
| Pattern complexe dans le texte | Regex |
| Performance critique | CSS (plus rapide) |
