# Tracking Plan — Ecommerce GA4

## 1. Contexto de negocio

| Campo | Detalle |
|-------|---------|
| **Tipo de negocio** | Ecommerce (tienda online con catálogo de productos) |
| **Plataforma** | Web (responsive) |
| **Herramientas** | Google Analytics 4 + Google Tag Manager (web container) |
| **Objetivo principal** | Maximizar ingresos optimizando el funnel de compra |
| **Fecha** | Marzo 2026 |
| **Autor** | Pablo Mantero Sierra |

## 2. KPIs principales

| KPI | Descripción | Fórmula / Fuente |
|-----|-------------|------------------|
| **Tasa de conversión** | % de sesiones que terminan en compra | `purchases / sessions × 100` |
| **Ingresos** | Facturación total | Suma de `value` en evento `purchase` |
| **AOV (Average Order Value)** | Valor medio por pedido | `ingresos / nº de transacciones` |
| **Tasa de abandono de carrito** | % de usuarios que añaden al carrito pero no compran | `1 - (purchases / add_to_cart) × 100` |
| **Revenue por usuario** | Ingresos medios por usuario | `ingresos / active users` |

## 3. Mapa de eventos

### 3.1 Eventos de descubrimiento de producto

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| `view_item_list` | Se muestra un listado de productos (categoría, búsqueda, recomendados) | `item_list_id`, `item_list_name`, `items[]` | PLP, búsqueda, home | Alta |
| `select_item` | Clic en un producto desde un listado | `item_list_id`, `item_list_name`, `items[]` (solo el producto clicado) | PLP, búsqueda, home | Media |
| `view_item` | Se carga la página de detalle de producto | `currency`, `value`, `items[]` | PDP | Alta |

### 3.2 Eventos del carrito

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| `add_to_cart` | Clic en "Añadir al carrito" | `currency`, `value`, `items[]` | PDP, PLP (si hay botón directo) | Crítica |
| `remove_from_cart` | Clic en "Eliminar" dentro del carrito | `currency`, `value`, `items[]` | Carrito | Alta |
| `view_cart` | Se abre o se carga la página del carrito | `currency`, `value`, `items[]` | Carrito | Alta |

### 3.3 Eventos de checkout

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| `begin_checkout` | El usuario inicia el proceso de checkout | `currency`, `value`, `coupon`, `items[]` | Checkout - paso 1 | Crítica |
| `add_shipping_info` | El usuario completa la información de envío | `currency`, `value`, `coupon`, `shipping_tier`, `items[]` | Checkout - envío | Alta |
| `add_payment_info` | El usuario introduce datos de pago | `currency`, `value`, `coupon`, `payment_type`, `items[]` | Checkout - pago | Alta |
| `purchase` | Confirmación de compra exitosa | `transaction_id`, `value`, `tax`, `shipping`, `currency`, `coupon`, `items[]` | Thank you page | Crítica |

### 3.4 Eventos de interacción

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| `search` | El usuario realiza una búsqueda | `search_term` | Cualquiera (barra de búsqueda) | Alta |
| `view_promotion` | Se muestra un banner o promoción | `creative_name`, `creative_slot`, `promotion_id`, `promotion_name`, `items[]` | Home, PLP | Media |
| `select_promotion` | Clic en un banner o promoción | `creative_name`, `creative_slot`, `promotion_id`, `promotion_name`, `items[]` | Home, PLP | Media |
| `add_to_wishlist` | Clic en "Añadir a favoritos" | `currency`, `value`, `items[]` | PDP, PLP | Baja |

### 3.5 Eventos personalizados (no estándar GA4)

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| `newsletter_signup` | Envío del formulario de newsletter | `method` (footer, popup, checkout) | Cualquiera | Media |
| `filter_applied` | El usuario aplica un filtro de producto | `filter_type` (color, talla, precio), `filter_value` | PLP | Media |
| `sort_applied` | El usuario cambia el orden de productos | `sort_type` (precio_asc, precio_desc, relevancia, novedades) | PLP | Baja |
| `size_guide_opened` | Clic en "Guía de tallas" | `item_id`, `item_category` | PDP | Baja |

## 4. Estructura del objeto `items[]`

Todos los eventos de ecommerce que incluyen productos usan el array `items[]`. Cada objeto dentro del array tiene esta estructura:

| Parámetro | Tipo | Obligatorio | Ejemplo | Descripción |
|-----------|------|-------------|---------|-------------|
| `item_id` | string | ✅ | `"SKU-12345"` | Identificador único del producto (SKU) |
| `item_name` | string | ✅ | `"Camiseta Básica Algodón"` | Nombre del producto |
| `item_brand` | string | Recomendado | `"MarcaX"` | Marca del producto |
| `item_category` | string | Recomendado | `"Ropa"` | Categoría principal |
| `item_category2` | string | Opcional | `"Camisetas"` | Subcategoría nivel 2 |
| `item_category3` | string | Opcional | `"Manga Corta"` | Subcategoría nivel 3 |
| `item_variant` | string | Opcional | `"Azul / M"` | Variante (color, talla, etc.) |
| `price` | number | ✅ | `29.95` | Precio unitario |
| `quantity` | number | ✅ | `1` | Cantidad |
| `discount` | number | Opcional | `5.00` | Descuento aplicado al producto |
| `coupon` | string | Opcional | `"VERANO20"` | Cupón aplicado al producto |
| `index` | number | Opcional | `0` | Posición del producto en el listado |

## 5. Ejemplos de dataLayer push

### view_item (página de producto)

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({ ecommerce: null }); // Limpiar ecommerce previo
window.dataLayer.push({
  event: "view_item",
  ecommerce: {
    currency: "EUR",
    value: 29.95,
    items: [{
      item_id: "SKU-12345",
      item_name: "Camiseta Básica Algodón",
      item_brand: "MarcaX",
      item_category: "Ropa",
      item_category2: "Camisetas",
      item_variant: "Azul / M",
      price: 29.95,
      quantity: 1
    }]
  }
});
```

### add_to_cart

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "add_to_cart",
  ecommerce: {
    currency: "EUR",
    value: 29.95,
    items: [{
      item_id: "SKU-12345",
      item_name: "Camiseta Básica Algodón",
      item_brand: "MarcaX",
      item_category: "Ropa",
      item_category2: "Camisetas",
      item_variant: "Azul / M",
      price: 29.95,
      quantity: 1
    }]
  }
});
```

### purchase (confirmación de compra)

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "purchase",
  ecommerce: {
    transaction_id: "TXN-98765",
    value: 74.85,
    tax: 15.72,
    shipping: 4.95,
    currency: "EUR",
    coupon: "VERANO20",
    items: [
      {
        item_id: "SKU-12345",
        item_name: "Camiseta Básica Algodón",
        item_brand: "MarcaX",
        item_category: "Ropa",
        item_category2: "Camisetas",
        item_variant: "Azul / M",
        price: 29.95,
        discount: 5.00,
        quantity: 2
      },
      {
        item_id: "SKU-67890",
        item_name: "Pantalón Chino Slim",
        item_brand: "MarcaY",
        item_category: "Ropa",
        item_category2: "Pantalones",
        item_variant: "Negro / 42",
        price: 49.95,
        quantity: 1
      }
    ]
  }
});
```

> **Nota importante:** Siempre ejecuta `dataLayer.push({ ecommerce: null })` antes de cada push de ecommerce. Esto limpia el objeto ecommerce anterior y evita que datos de un evento se mezclen con el siguiente. Es una de las fuentes de errores más comunes.

## 6. Dimensiones personalizadas

| Dimensión | Scope | Parámetro del evento | Uso |
|-----------|-------|---------------------|-----|
| `user_type` | User | `user_type` | Diferenciar "nuevo" vs "recurrente" (lógica de negocio, no la de GA4) |
| `login_status` | Session | `login_status` | Saber si el usuario está logueado o es guest |
| `page_type` | Event | `page_type` | Clasificar páginas: home, PLP, PDP, checkout, etc. |
| `item_stock_status` | Event | `item_stock_status` | Saber si el producto estaba en stock cuando se visualizó |

## 7. Key Events (Conversiones)

| Key Event | Evento base | Condiciones | Valor |
|-----------|-------------|-------------|-------|
| **Compra** | `purchase` | Ninguna (todas las compras) | Dinámico (`value`) |
| **Inicio de checkout** | `begin_checkout` | Ninguna | No |
| **Añadir al carrito** | `add_to_cart` | Ninguna | No |
| **Newsletter** | `newsletter_signup` | Ninguna | No |

> **Criterio:** Solo marco como Key Event los eventos que representan una acción de valor clara para el negocio. Marcar demasiados eventos como conversión diluye el análisis.

## 8. Audiencias sugeridas

| Audiencia | Definición | Uso |
|-----------|------------|-----|
| Compradores recurrentes | `purchase` count > 1 en 90 días | Campañas de fidelización |
| Abandonadores de carrito | `add_to_cart` SIN `purchase` en 7 días | Remarketing |
| Usuarios de alto valor | `purchase` con `value` > percentil 75 | Lookalike audiences |
| Buscadores sin compra | `search` SIN `purchase` en sesión | Optimización de búsqueda interna |

## 9. Notas de implementación para desarrollo

1. **El dataLayer debe cargarse ANTES de la etiqueta de GTM.** Cualquier push que se haga después de que GTM esté listo se recoge, pero los push previos al snippet deben estar en el HTML antes del script de GTM.
2. **Los valores numéricos (`value`, `price`, `tax`, `shipping`) deben ser tipo number, no string.** `29.95` es correcto, `"29.95"` no.
3. **`transaction_id` debe ser único por pedido.** Si se envía duplicado, GA4 lo deduplica, pero es mejor evitarlo desde origen.
4. **Testear siempre con GTM Preview y GA4 DebugView** antes de publicar cualquier cambio.
5. **El `currency` debe seguir el formato ISO 4217** (EUR, USD, GBP...) y debe estar presente en todos los eventos que incluyan `value`.

## 10. QA Checklist

Antes de dar por buena la implementación, verificar:

- [ ] Todos los eventos del plan se disparan en las páginas correctas
- [ ] Los parámetros llegan con el tipo de dato correcto (string, number)
- [ ] `ecommerce: null` se ejecuta antes de cada push
- [ ] `transaction_id` es único en cada `purchase`
- [ ] El `value` del `purchase` coincide con el importe real del pedido
- [ ] Los `items[]` contienen todos los productos de la transacción
- [ ] Las dimensiones personalizadas se registran en GA4 (Admin → Custom definitions)
- [ ] Los Key Events están marcados en GA4 (Admin → Key events)
- [ ] No se disparan eventos duplicados (especialmente `purchase`)
- [ ] Consent Mode funciona correctamente (los eventos solo se disparan con consentimiento)
