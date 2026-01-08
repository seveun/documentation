# Patterns d'URL (Minimatch)

Ces patterns servent a filtrer les URLs que le crawler va explorer. Ils utilisent la syntaxe **glob** (comme les wildcards).

---

## Comment ca fonctionne

Dans le formulaire de configuration du crawl, vous avez 3 champs de filtrage d'URL :

| Champ | Comportement |
|-------|-------------|
| **Include Patterns** | Le crawler ne suit QUE les URLs qui correspondent a TOUS les patterns |
| **Exclude Patterns** | Le crawler ignore les URLs qui correspondent a AU MOINS UN pattern |
| **Subdomains Patterns** | Autorise le crawl de sous-domaines specifiques |

**Important** : Ces patterns s'appliquent au **chemin** de l'URL (apres le domaine), pas au domaine lui-meme.

Chaque champ permet d'ajouter plusieurs patterns (bouton +). Entrez un pattern par ligne.

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

| Objectif | Pattern a entrer |
|----------|------------------|
| Tout le blog | `**/blog/**` |
| Pages produits | `**/product/**` |
| Version francaise | `**/fr/**` |
| Categorie specifique | `**/category/chaussures/**` |

---

## Patterns d'exclusion

Avec plusieurs patterns d'exclusion, **UN SEUL suffit** pour exclure (OR).

| Objectif | Pattern a entrer |
|----------|------------------|
| Exclure PDFs | `**/*.pdf` |
| Exclure pagination | `**/page/[2-9]/**` |
| Exclure pagination 10+ | `**/page/[1-9][0-9]/**` |
| Exclure recherche | `**/search/**` |
| Exclure recherche (param) | `**?s=**` |
| Exclure admin | `**/admin/**` |
| Exclure wp-admin | `**/wp-admin/**` |
| Exclure panier | `**/cart/**` |
| Exclure checkout | `**/checkout/**` |
| Exclure compte | `**/account/**` |
| Exclure tri | `**?*sort=**` |
| Exclure filtres | `**?*orderby=**` |

---

## Patterns de sous-domaines

Par defaut, le crawler reste sur le domaine principal. Le pattern matche la partie **AVANT** le domaine.

| Objectif | Pattern a entrer |
|----------|------------------|
| Sous-domaine blog | `blog.` |
| Sous-domaine shop | `shop.` |
| Tous sous-domaines | `*` |

**Attention** : N'oubliez pas le point final (`blog.` et non `blog`).

Pour les langues EU, ajoutez un pattern par sous-domaine :
- `fr.`
- `en.`
- `de.`
- `es.`
- `it.`

---

## Comprendre la logique Include / Exclude

**Important** : Si vous definissez un Include Pattern, seules les URLs qui matchent seront crawlees. Tout le reste est **automatiquement exclu**.

Les Exclude Patterns servent uniquement a :
1. **Affiner** ce qui est deja inclus (ex: blog sauf pagination)
2. **Filtrer un crawl complet** quand vous n'avez pas d'Include Pattern

---

## Cas d'application

### 1. Audit SEO d'un blog

**Include Patterns** :
| Pattern |
|---------|
| `**/blog/**` |

C'est tout ! Les pages `/cart/`, `/admin/`, etc. ne matchent pas `**/blog/**` donc elles sont deja exclues.

Pour affiner (optionnel) - exclure pagination et pages auteur du blog :

**Exclude Patterns** :
| Pattern |
|---------|
| `**/page/[2-9]/**` |
| `**/page/[1-9][0-9]/**` |
| `**/author/**` |

---

### 2. Audit e-commerce - Fiches produits uniquement

**Include Patterns** :
| Pattern |
|---------|
| `**/product/**` |

Pour affiner (optionnel) - exclure les variantes avec filtres :

**Exclude Patterns** :
| Pattern |
|---------|
| `**?*sort=**` |
| `**?*filter=**` |

---

### 3. Site multilingue - Version francaise uniquement

**Include Patterns** :
| Pattern |
|---------|
| `**/fr/**` |

Ou si le site utilise des sous-domaines :

**Subdomains Patterns** :
| Pattern |
|---------|
| `fr.` |

---

### 4. Site multilingue - Toutes les versions EU

**Subdomains Patterns** :
| Pattern |
|---------|
| `fr.` |
| `de.` |
| `es.` |
| `it.` |
| `uk.` |
| `nl.` |
| `be.` |

---

### 5. Crawl complet SAUF pages a faible valeur SEO

Quand vous voulez crawler tout le site mais exclure certaines sections :

**Exclude Patterns** :
| Pattern |
|---------|
| `**/search/**` |
| `**/recherche/**` |
| `**?s=**` |
| `**/tag/**` |
| `**/author/**` |
| `**/page/[2-9]/**` |
| `**/page/[1-9][0-9]/**` |
| `**/admin/**` |
| `**/wp-admin/**` |
| `**/cart/**` |
| `**/checkout/**` |
| `**/account/**` |
| `**/login/**` |
| `**/*.pdf` |

---

### 6. Crawler le blog SAUF une annee specifique

**Include Patterns** :
| Pattern |
|---------|
| `**/blog/**` |

**Exclude Patterns** :
| Pattern |
|---------|
| `**/blog/2020/**` |

---

### 7. Crawler uniquement les pages de categorie (sans pagination)

**Include Patterns** :
| Pattern |
|---------|
| `**/category/**` |

**Exclude Patterns** :
| Pattern |
|---------|
| `**/page/[2-9]/**` |

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

Pour tester vos patterns avant de les rentrer sur le front :
- [regex101.com](https://regex101.com/) - Testeur de patterns en ligne
