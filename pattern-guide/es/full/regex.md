# Expresiones Regulares (Regex)

Las regex extraen texto buscando patrones en el codigo fuente HTML.

---

## Como funciona

Escribe tu regex directamente en el campo **Extraction Pattern** (sin prefijo).

**Importante**:
- NO envuelvas con `/`
- Usa **grupos de captura** `()` para extraer partes especificas
- El flag `g` (global) se aplica automaticamente

---

## Sintaxis basica

### Caracteres especiales

| Patron | Significado | Ejemplo |
|--------|-------------|---------|
| `.` | Cualquier caracter | `a.c` → "abc", "a1c" |
| `\d` | Un digito (0-9) | `\d+` → "123" |
| `\D` | No es digito | `\D+` → "abc" |
| `\w` | Letra, digito o _ | `\w+` → "hello_123" |
| `\W` | No es palabra | `\W+` → "!@#" |
| `\s` | Espacio, tab, salto | `hello\s+world` |
| `\S` | No es espacio | `\S+` → "hello" |

### Cuantificadores

| Patron | Significado | Ejemplo |
|--------|-------------|---------|
| `*` | 0 o mas | `ab*c` → "ac", "abc", "abbc" |
| `+` | 1 o mas | `ab+c` → "abc", "abbc" (no "ac") |
| `?` | 0 o 1 | `colou?r` → "color", "colour" |
| `{n}` | Exactamente n veces | `\d{5}` → "12345" |
| `{n,}` | Al menos n veces | `\d{2,}` → "12", "123", "1234" |
| `{n,m}` | Entre n y m | `\d{2,4}` → "12", "123", "1234" |

### Clases de caracteres

| Patron | Significado | Ejemplo |
|--------|-------------|---------|
| `[abc]` | Uno de a, b, c | `[aeiou]` → vocales |
| `[^abc]` | No a, b, c | `[^0-9]` → no digito |
| `[a-z]` | Rango | `[a-zA-Z]` → letras |
| `[0-9]` | Igual que `\d` | `[0-9]+` → numeros |

### Anclas y limites

| Patron | Significado | Ejemplo |
|--------|-------------|---------|
| `^` | Inicio de linea | `^Titulo` |
| `$` | Fin de linea | `\.html$` |
| `\b` | Limite de palabra | `\bprecio\b` → "precio" solo |

### Grupos

| Patron | Significado | Ejemplo |
|--------|-------------|---------|
| `(...)` | Grupo de captura | `precio: (\d+)` → captura numero |
| `(?:...)` | Grupo sin captura | `(?:www\.)?` |
| `\|` | Alternativa (O) | `(gato\|perro)` |

---

## Casos de uso

### 1. Extraer precios

| Metodo | Extraction Pattern |
|--------|-------------------|
| Formato europeo (29,99 €) | `(\d+[,.]\d{2})\s*€` |
| Formato US ($29.99) | `\$(\d+\.\d{2})` |
| Con EUR | `(\d+[,.]\d{2})\s*EUR` |

### 2. Extraer numeros de telefono

| Metodo | Extraction Pattern |
|--------|-------------------|
| Formato espanol | `(\+34[\s.-]?[6-9]\d{2}[\s.-]?\d{3}[\s.-]?\d{3})` |
| Formato fijo | `(9[0-9]{2}[\s.-]?\d{2}[\s.-]?\d{2}[\s.-]?\d{2})` |

Coincide: "+34 612 345 678", "+34612345678"

### 3. Extraer emails

| Metodo | Extraction Pattern |
|--------|-------------------|
| Email estandar | `([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` |

### 4. Extraer codigos postales

| Metodo | Extraction Pattern |
|--------|-------------------|
| Espana (5 digitos) | `\b(\d{5})\b` |

### 5. Extraer fechas

| Metodo | Extraction Pattern |
|--------|-------------------|
| Formato DD/MM/AAAA | `(\d{2}/\d{2}/\d{4})` |
| Formato AAAA-MM-DD | `(\d{4}-\d{2}-\d{2})` |
| Formato textual | `(\d{1,2}\s+de\s+(?:enero\|febrero\|marzo\|abril\|mayo\|junio\|julio\|agosto\|septiembre\|octubre\|noviembre\|diciembre)\s+de\s+\d{4})` |

### 6. Extraer referencias de producto

| Metodo | Extraction Pattern |
|--------|-------------------|
| Formato ABC-12345 | `([A-Z]{2,4}-\d{4,6})` |
| Formato EAN/GTIN | `\b(\d{13})\b` |

### 7. Extraer datos estructurados

| Metodo | Extraction Pattern |
|--------|-------------------|
| Tipos schema.org | `"@type"\s*:\s*"([^"]+)"` |

### 8. Extraer meta tags especificos

| Metodo | Extraction Pattern |
|--------|-------------------|
| Meta author | `<meta[^>]*name=["']author["'][^>]*content=["']([^"']+)["']` |
| Meta robots | `<meta[^>]*name=["']robots["'][^>]*content=["']([^"']+)["']` |

### 9. Extraer data-attributes

| Metodo | Extraction Pattern |
|--------|-------------------|
| data-product-id | `data-product-id=["'](\d+)["']` |
| data-price | `data-price=["']([\d.,]+)["']` |

### 10. Extraer URLs

| Metodo | Extraction Pattern |
|--------|-------------------|
| URLs de imagenes | `<img[^>]+src=["']([^"']+)["']` |
| URLs de enlaces | `<a[^>]+href=["']([^"']+)["']` |

---

## Errores comunes

| Error | Correccion |
|-------|------------|
| `/\d+/` | `\d+` (sin barras) |
| `precio: \d+` | `precio: (\d+)` (grupo de captura) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |
| `10.99` | `10\.99` (escapar punto) |
| `$10` | `\$10` (escapar dolar) |

---

## Consejos

### Probar tus regex

Usa [regex101.com](https://regex101.com) con modo **ECMAScript/JavaScript**.

### Regex non-greedy

Anade `?` despues de cuantificadores para coincidir minimo:
- `.*` → greedy (todo hasta ultimo match)
- `.*?` → non-greedy (todo hasta primer match)

### Grupos de captura

Solo el contenido del **primer grupo** `()` se extrae. Usa `(?:...)` para grupos sin captura.

```
Regex: precio:\s*(\d+)\s*(?:€|EUR)
Captura: solo el numero
```
