# Regex Cheatsheet para Google Tag Manager

Patrones de expresiones regulares que uso habitualmente en GTM para triggers, variables y filtros.

## Conceptos básicos

| Símbolo | Significado | Ejemplo |
|---------|-------------|---------|
| `.` | Cualquier carácter | `a.c` → abc, aXc, a1c |
| `*` | 0 o más repeticiones del anterior | `ab*c` → ac, abc, abbc |
| `+` | 1 o más repeticiones del anterior | `ab+c` → abc, abbc (no ac) |
| `?` | 0 o 1 repetición del anterior | `colou?r` → color, colour |
| `^` | Inicio de cadena | `^/blog` → /blog/post (no /es/blog) |
| `$` | Fin de cadena | `\.pdf$` → archivo.pdf |
| `\|` | OR (alternativa) | `cat\|dog` → cat, dog |
| `()` | Grupo de captura | `(ab)+` → ab, abab |
| `[]` | Clase de caracteres | `[aeiou]` → cualquier vocal |
| `\d` | Cualquier dígito | `\d{3}` → 123, 456 |
| `\w` | Letra, dígito o guion bajo | `\w+` → hola_mundo |
| `\s` | Espacio en blanco | `\s+` → uno o más espacios |
| `\` | Escape de carácter especial | `\.` → un punto literal |

## Patrones para URLs (Page Path triggers)

### Páginas de producto (PDP)

```regex
^/producto/[^/]+/?$
```
Coincide con: `/producto/camiseta-azul`, `/producto/camiseta-azul/`
No coincide con: `/producto/`, `/producto/camiseta/reviews`

### Páginas de categoría (PLP)

```regex
^/categoria/[^/]+/?$
```

### Checkout (cualquier paso)

```regex
^/checkout
```
Coincide con: `/checkout`, `/checkout/envio`, `/checkout/pago`

### Thank you page

```regex
^/checkout/gracias
```

### Blog (cualquier post)

```regex
^/blog/.+
```
Coincide con: `/blog/mi-articulo`, `/blog/2026/post`
No coincide con: `/blog` o `/blog/` (el listado)

### Excluir entornos de desarrollo/staging

```regex
^(?!.*(staging|dev|test|localhost)).*$
```

## Patrones para parámetros de URL

### Extraer UTM source

Variable de tipo "Regex Table" en GTM:

| Input Variable | `{{Page URL}}` |
|---|---|
| Pattern | `[?&]utm_source=([^&]+)` |
| Output | `$1` |

### Extraer ID de producto de la URL

```regex
/producto/([^/?]+)
```
De `/producto/SKU-12345?color=azul` extrae `SKU-12345`

### Detectar si la URL tiene query parameters

```regex
\?.*=
```

## Patrones para Click URLs y Click Text

### Clics en archivos descargables

```regex
\.(pdf|xlsx?|docx?|pptx?|csv|zip)(\?.*)?$
```

### Clics en enlaces externos (outbound)

```regex
^https?://(?!tu-dominio\.com)
```
Reemplaza `tu-dominio\.com` con tu dominio.

### Clics en enlaces de email

```regex
^mailto:
```

### Clics en enlaces de teléfono

```regex
^tel:
```

### Clics en botones de "Añadir al carrito" (por texto)

```regex
^(añadir al carrito|add to cart|agregar al carro)$
```
Usa flag `i` (case insensitive) si tu GTM lo permite, o usa:
```regex
^[Aa](ñadir al carrito|dd to cart)
```

## Patrones para filtros en GA4

### Filtrar tráfico interno por IP

En GA4 (Admin → Data Streams → Configurar → Define internal traffic):

```regex
^192\.168\.1\.\d{1,3}$
```
Para un rango tipo 192.168.1.x

```regex
^(192\.168\.1\.\d{1,3}|10\.0\.0\.\d{1,3})$
```
Para múltiples rangos.

### Filtrar referrals internos (cross-domain)

```regex
dominio1\.com|dominio2\.com|pago\.dominio1\.com
```

## Patrones para validación

### Validar formato de email (simplificado)

```regex
^[^\s@]+@[^\s@]+\.[^\s@]+$
```

### Validar formato de SKU (ejemplo: SKU-XXXXX)

```regex
^SKU-\d{5}$
```

### Validar transaction_id (ejemplo: TXN-YYYY-NNNNN)

```regex
^TXN-\d{4}-\d{5}$
```

## Tips para regex en GTM

1. **GTM usa JavaScript regex.** La mayoría de patrones estándar funcionan, pero hay algunas limitaciones (no soporta lookbehind en navegadores antiguos).
2. **En triggers de "Page Path matches RegEx"**, no necesitas poner delimitadores (`/`). Solo el patrón.
3. **El match es parcial por defecto.** Si quieres match exacto, usa `^` al inicio y `$` al final.
4. **Testea siempre en [regex101.com](https://regex101.com)** antes de meter el patrón en GTM. Selecciona "JavaScript" como flavor.
5. **En Regex Tables**, el grupo de captura `$1` devuelve lo que está entre el primer `()`.
