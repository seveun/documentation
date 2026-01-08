# Selecteurs CSS

Les selecteurs CSS permettent d'extraire des elements HTML specifiques de chaque page crawlee.

---

## Comment ca fonctionne

Entrez votre selecteur CSS dans le champ **Extraction Pattern**. Le crawler extrait le **texte** de tous les elements qui matchent.

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

| Methode | Extraction Pattern |
|---------|-------------------|
| Par classe | `.price` |
| Par classe partielle | `[class*="price"]` |
| Dans un conteneur | `.product .price` |

### 2. Extraire les titres de produits

| Methode | Extraction Pattern |
|---------|-------------------|
| H1 avec classe | `h1.product-title` |
| Classe partielle | `[class*="product-title"]` |

### 3. Extraire les references/SKU

| Methode | Extraction Pattern |
|---------|-------------------|
| Via data-attribute | `[data-sku]` |
| Via classe | `.product-reference` |

### 4. Extraire les notes/avis

| Methode | Extraction Pattern |
|---------|-------------------|
| Classe partielle | `[class*="rating"]` |
| Classe exacte | `.review-score` |

### 5. Extraire le contenu principal

| Methode | Extraction Pattern |
|---------|-------------------|
| Balise semantique | `main` |
| Par ID | `#content` |
| Balise article | `article` |

### 6. Extraire les dates de publication

| Methode | Extraction Pattern |
|---------|-------------------|
| Balise time | `time[datetime]` |
| Par classe | `.post-date` |

### 7. Extraire les categories

| Methode | Extraction Pattern |
|---------|-------------------|
| Breadcrumb | `.breadcrumb a` |
| Classe partielle | `[class*="category"] a` |

### 8. Extraire les auteurs

| Methode | Extraction Pattern |
|---------|-------------------|
| Par classe | `.author-name` |
| Par attribut rel | `[rel="author"]` |

### 9. Extraire la disponibilite produit

| Methode | Extraction Pattern |
|---------|-------------------|
| Disponibilite | `[class*="availability"]` |
| Stock | `[class*="stock"]` |

### 10. Extraire les meta tags

| Methode | Extraction Pattern |
|---------|-------------------|
| Meta description | `meta[name="description"]` |
| Meta robots | `meta[name="robots"]` |

---

## Erreurs courantes

| Erreur | Correction |
|--------|------------|
| `.price product` | `.price.product` ou `.price .product` |
| `div[class=price]` | `div[class*="price"]` |

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
