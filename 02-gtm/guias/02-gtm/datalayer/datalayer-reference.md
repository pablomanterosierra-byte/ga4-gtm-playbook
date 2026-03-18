# dataLayer Reference — Ecommerce GA4

Referencia rápida de todos los eventos y parámetros del dataLayer de ecommerce para GA4.

## Índice de eventos

| Fase del funnel | Evento | Cuándo se dispara |
|----------------|--------|-------------------|
| Descubrimiento | `view_item_list` | Se muestra un listado de productos |
| Descubrimiento | `select_item` | Clic en un producto del listado |
| Descubrimiento | `view_item` | Se carga la ficha de producto |
| Descubrimiento | `view_promotion` | Se muestra un banner/promoción |
| Descubrimiento | `select_promotion` | Clic en un banner/promoción |
| Carrito | `add_to_cart` | Clic en "Añadir al carrito" |
| Carrito | `remove_from_cart` | Clic en "Eliminar" del carrito |
| Carrito | `view_cart` | Se abre la página del carrito |
| Checkout | `begin_checkout` | Se inicia el proceso de compra |
| Checkout | `add_shipping_info` | Se completa info de envío |
| Checkout | `add_payment_info` | Se introduce método de pago |
| Conversión | `purchase` | Compra confirmada |
| Post-venta | `refund` | Reembolso procesado |
| Engagement | `add_to_wishlist` | Producto añadido a favoritos |
| Engagement | `search` | Búsqueda interna |

## Parámetros por evento

### Nivel de evento (fuera de `items[]`)

| Parámetro | Tipo | Eventos donde aplica |
|-----------|------|---------------------|
| `currency` | string (ISO 4217) | Todos los que incluyen `value` |
| `value` | number | `view_item`, `add_to_cart`, `remove_from_cart`, `view_cart`, `begin_checkout`, `add_shipping_info`, `add_payment_info`, `purchase` |
| `transaction_id` | string | `purchase`, `refund` |
| `tax` | number | `purchase` |
| `shipping` | number | `purchase` |
| `coupon` | string | `begin_checkout`, `add_shipping_info`, `add_payment_info`, `purchase` |
| `shipping_tier` | string | `add_shipping_info` |
| `payment_type` | string | `add_payment_info` |
| `item_list_id` | string | `view_item_list`, `select_item` |
| `item_list_name` | string | `view_item_list`, `select_item` |
| `creative_name` | string | `view_promotion`, `select_promotion` |
| `creative_slot` | string | `view_promotion`, `select_promotion` |
| `promotion_id` | string | `view_promotion`, `select_promotion` |
| `promotion_name` | string | `view_promotion`, `select_promotion` |
| `search_term` | string | `search` |

### Nivel de item (dentro de `items[]`)

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|-------------|-------------|
| `item_id` | string | ✅ | SKU o identificador único |
| `item_name` | string | ✅ | Nombre del producto |
| `item_brand` | string | Recomendado | Marca |
| `item_category` | string | Recomendado | Categoría nivel 1 |
| `item_category2` | string | Opcional | Categoría nivel 2 |
| `item_category3` | string | Opcional | Categoría nivel 3 |
| `item_category4` | string | Opcional | Categoría nivel 4 |
| `item_category5` | string | Opcional | Categoría nivel 5 |
| `item_variant` | string | Opcional | Variante (color, talla...) |
| `price` | number | ✅ | Precio unitario |
| `quantity` | number | ✅ | Cantidad |
| `discount` | number | Opcional | Descuento aplicado |
| `coupon` | string | Opcional | Cupón a nivel de producto |
| `index` | number | Opcional | Posición en el listado |
| `affiliation` | string | Opcional | Tienda o canal de venta |
| `item_list_id` | string | Opcional | ID del listado de origen |
| `item_list_name` | string | Opcional | Nombre del listado de origen |

## Cálculo del `value`

El parámetro `value` debe representar el valor monetario real del evento:

| Evento | Cómo calcular `value` |
|--------|----------------------|
| `view_item` | Precio del producto: `price × quantity` |
| `add_to_cart` | Precio de lo añadido: `price × quantity` |
| `remove_from_cart` | Precio de lo eliminado: `price × quantity` |
| `view_cart` | Total del carrito: suma de `price × quantity` de cada item |
| `begin_checkout` | Total del checkout: suma de items (sin shipping ni tax) |
| `purchase` | Total final: suma de items con descuentos aplicados (sin shipping ni tax, o con ellos si tu lógica de negocio lo requiere — documenta el criterio) |

> **Consistencia:** Decide un criterio para `value` en `purchase` (con o sin shipping/tax) y mantenlo. Lo importante es que sea consistente, no cuál elijas.
