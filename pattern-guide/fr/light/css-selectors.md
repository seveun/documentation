# Selecteurs CSS - Aide-memoire

## Syntaxe

| Selecteur | Description |
|-----------|-------------|
| `h1` | Par balise |
| `.classe` | Par classe |
| `#id` | Par ID |
| `[attr]` | Par attribut |
| `[attr="val"]` | Attribut = valeur |
| `[attr*="val"]` | Attribut contient |
| `.parent .enfant` | Descendant |
| `.parent > .enfant` | Enfant direct |

## Exemples rapides

| Objectif | Extraction Pattern |
|----------|-------------------|
| Prix | `.price` |
| Prix (classe partielle) | `[class*="price"]` |
| Titre produit | `h1.product-title` |
| Date publication | `time[datetime]` |
| SKU | `[data-sku]` |
| Contenu principal | `main` |
| Categories | `.breadcrumb a` |
| Auteur | `.author-name` |

## Erreurs frequentes

| Faux | Juste |
|------|-------|
| `.a .b` (espace mal compris) | `.a.b` (meme element) ou `.a .b` (descendant) |

## Test

Console navigateur : `document.querySelectorAll('.votre-selecteur')`
