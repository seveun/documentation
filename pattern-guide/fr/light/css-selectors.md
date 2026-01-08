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

```json
// Prix
{ "extractPattern": ".price", "extractName": "prix" }

// Prix (classe partielle)
{ "extractPattern": "[class*=\"price\"]", "extractName": "prix" }

// Titre produit
{ "extractPattern": "h1.product-title", "extractName": "titre" }

// Date publication
{ "extractPattern": "time[datetime]", "extractName": "date" }

// SKU via data-attribute
{ "extractPattern": "[data-sku]", "extractName": "sku" }

// Contenu principal
{ "extractPattern": "main", "extractName": "contenu" }
```

## Erreurs frequentes

| Faux | Juste |
|------|-------|
| `.a .b` (espace mal compris) | `.a.b` (meme element) ou `.a .b` (descendant) |

## Test

Console navigateur : `document.querySelectorAll('.votre-selecteur')`
