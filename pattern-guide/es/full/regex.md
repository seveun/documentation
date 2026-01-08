# Expresiones Regulares (Regex)

Las regex extraen texto buscando patrones en el codigo fuente HTML.

---

## Como funciona

Escribe tu regex directamente en `extractPattern` (sin prefijo).

```json
{
  "extractPattern": "(\\d+[,.]\\d{2})\\s*€",
  "extractName": "precio"
}
```

**Importante**:
- NO envuelvas con `/`
- Usa **grupos de captura** `()` para extraer partes especificas
- El flag `g` (global) se aplica automaticamente
- Escapa las barras invertidas: `\d` se convierte en `\\d` en JSON

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

**Formato europeo (29,99 €):**
```json
{
  "extractPattern": "(\\d+[,.]\\d{2})\\s*€",
  "extractName": "precio"
}
```

**Formato US ($29.99):**
```json
{
  "extractPattern": "\\$(\\d+\\.\\d{2})",
  "extractName": "price"
}
```

**Con EUR:**
```json
{
  "extractPattern": "(\\d+[,.]\\d{2})\\s*EUR",
  "extractName": "precio"
}
```

### 2. Extraer numeros de telefono

**Formato espanol:**
```json
{
  "extractPattern": "(\\+34[\\s.-]?[6-9]\\d{2}[\\s.-]?\\d{3}[\\s.-]?\\d{3})",
  "extractName": "telefono"
}
```
Coincide: "+34 612 345 678", "+34612345678"

**Formato fijo:**
```json
{
  "extractPattern": "(9[0-9]{2}[\\s.-]?\\d{2}[\\s.-]?\\d{2}[\\s.-]?\\d{2})",
  "extractName": "telefono"
}
```

### 3. Extraer emails

```json
{
  "extractPattern": "([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,})",
  "extractName": "email"
}
```

### 4. Extraer codigos postales

**Espana (5 digitos):**
```json
{
  "extractPattern": "\\b(\\d{5})\\b",
  "extractName": "codigo_postal"
}
```

### 5. Extraer fechas

**Formato DD/MM/AAAA:**
```json
{
  "extractPattern": "(\\d{2}/\\d{2}/\\d{4})",
  "extractName": "fecha"
}
```

**Formato AAAA-MM-DD:**
```json
{
  "extractPattern": "(\\d{4}-\\d{2}-\\d{2})",
  "extractName": "fecha"
}
```

**Formato textual:**
```json
{
  "extractPattern": "(\\d{1,2}\\s+de\\s+(?:enero|febrero|marzo|abril|mayo|junio|julio|agosto|septiembre|octubre|noviembre|diciembre)\\s+de\\s+\\d{4})",
  "extractName": "fecha"
}
```

### 6. Extraer referencias de producto

**Formato ABC-12345:**
```json
{
  "extractPattern": "([A-Z]{2,4}-\\d{4,6})",
  "extractName": "referencia"
}
```

**Formato EAN/GTIN:**
```json
{
  "extractPattern": "\\b(\\d{13})\\b",
  "extractName": "ean"
}
```

### 7. Extraer datos estructurados

**Tipos schema.org:**
```json
{
  "extractPattern": "\"@type\"\\s*:\\s*\"([^\"]+)\"",
  "extractName": "schema_types"
}
```

### 8. Extraer meta tags especificos

**Contenido del meta author:**
```json
{
  "extractPattern": "<meta[^>]*name=[\"']author[\"'][^>]*content=[\"']([^\"']+)[\"']",
  "extractName": "autor"
}
```

**Contenido del meta robots:**
```json
{
  "extractPattern": "<meta[^>]*name=[\"']robots[\"'][^>]*content=[\"']([^\"']+)[\"']",
  "extractName": "meta_robots"
}
```

### 9. Extraer data-attributes

**data-product-id:**
```json
{
  "extractPattern": "data-product-id=[\"'](\\d+)[\"']",
  "extractName": "product_id"
}
```

**data-price:**
```json
{
  "extractPattern": "data-price=[\"']([\\d.,]+)[\"']",
  "extractName": "precio"
}
```

### 10. Extraer URLs

**URLs de imagenes:**
```json
{
  "extractPattern": "<img[^>]+src=[\"']([^\"']+)[\"']",
  "extractName": "imagenes"
}
```

**URLs de enlaces:**
```json
{
  "extractPattern": "<a[^>]+href=[\"']([^\"']+)[\"']",
  "extractName": "enlaces"
}
```

---

## Errores comunes

| Error | Correccion |
|-------|------------|
| `/\d+/` | `\d+` (sin barras) |
| `\d+` en JSON | `\\d+` (escapar barras) |
| `precio: \d+` | `precio: (\d+)` (grupo de captura) |
| `<div>.*</div>` | `<div>.*?</div>` (non-greedy) |
| `10.99` | `10\\.99` (escapar punto) |
| `$10` | `\\$10` (escapar dolar) |

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

### Escape en JSON

En JSON, duplica las barras invertidas:
```
Regex normal: \d+
En JSON:      \\d+
```
