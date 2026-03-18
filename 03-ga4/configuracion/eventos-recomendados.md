# Eventos recomendados en GA4 para Ecommerce

GA4 tiene tres niveles de eventos. Entender la diferencia es clave para saber qué implementar y cómo.

## Los tres niveles de eventos

### 1. Eventos automáticos (los recoge GA4 sin hacer nada)

| Evento | Qué mide |
|--------|----------|
| `first_visit` | Primera visita del usuario |
| `session_start` | Inicio de una sesión |
| `page_view` | Visualización de página |
| `scroll` | Scroll del 90% de la página |
| `click` (outbound) | Clics en enlaces que salen de tu dominio |
| `view_search_results` | Visualización de resultados del buscador del sitio |
| `file_download` | Descarga de archivos |
| `form_start` | Interacción inicial con un formulario |
| `form_submit` | Envío de un formulario |
| `video_start` | Inicio de reproducción de vídeo (YouTube embebido) |
| `video_progress` | 10%, 25%, 50%, 75% del vídeo |
| `video_complete` | Vídeo visto al completo |

> **Nota:** La mayoría se activan en Enhanced Measurement (Admin → Data Streams → Enhanced Measurement). Revisa que esté activado.

### 2. Eventos recomendados (los defines tú, pero GA4 los "entiende")

Estos son los eventos que Google espera para ecommerce. Si usas exactamente estos nombres y parámetros, GA4 genera informes especiales de ecommerce automáticamente.

| Evento | Descripción | Informe automático |
|--------|-------------|-------------------|
| `view_item_list` | Ver listado de productos | Sí - Product list performance |
| `select_item` | Clic en producto de listado | Sí |
| `view_item` | Ver ficha de producto | Sí - Product detail views |
| `add_to_cart` | Añadir al carrito | Sí - Ecommerce funnel |
| `remove_from_cart` | Quitar del carrito | Sí |
| `view_cart` | Ver carrito | Sí |
| `begin_checkout` | Empezar checkout | Sí - Ecommerce funnel |
| `add_shipping_info` | Añadir info de envío | Sí - Checkout funnel |
| `add_payment_info` | Añadir info de pago | Sí - Checkout funnel |
| `purchase` | Compra completada | Sí - Revenue, transactions |
| `refund` | Reembolso | Sí - Ajusta revenue |
| `view_promotion` | Ver promoción/banner | Sí - Promotion performance |
| `select_promotion` | Clic en promoción | Sí |
| `add_to_wishlist` | Añadir a favoritos | No |
| `search` | Búsqueda interna | Parcial (Search terms report) |

> **Regla de oro:** Usa siempre los nombres exactos. `addToCart` o `add-to-cart` no funcionan. El nombre correcto es `add_to_cart`.

### 3. Eventos personalizados (custom events)

Cualquier evento que no esté en las listas anteriores. Los defines tú con nombre y parámetros libres. GA4 no genera informes automáticos para estos, pero los puedes analizar en Explorations.

Ejemplos para ecommerce:

| Evento custom | Uso |
|---------------|-----|
| `newsletter_signup` | Suscripción a newsletter |
| `filter_applied` | Usuario aplica filtro en PLP |
| `sort_applied` | Usuario cambia orden en PLP |
| `size_guide_opened` | Usuario abre guía de tallas |
| `store_locator_used` | Usuario busca tienda física |
| `review_submitted` | Usuario envía una reseña |
| `coupon_applied` | Usuario aplica un cupón |

## Límites de GA4 a tener en cuenta

| Concepto | Límite |
|----------|--------|
| Tipos de eventos distintos | 500 por propiedad |
| Parámetros por evento | 25 |
| Longitud del nombre de evento | 40 caracteres |
| Longitud del nombre de parámetro | 40 caracteres |
| Longitud del valor de parámetro (string) | 100 caracteres |
| Custom dimensions (event-scoped) | 50 |
| Custom dimensions (user-scoped) | 25 |
| Custom metrics | 50 |
| Conversiones / Key Events | 30 |

> **Planifica con estos límites en mente.** 500 eventos parecen muchos, pero si no hay control, se llenan rápido con eventos mal nombrados o duplicados. El tracking plan es tu herramienta para evitarlo.

## Mi criterio para decidir qué implementar

No todos los eventos tienen la misma prioridad. Mi recomendación para ecommerce:

**Imprescindibles (día 1):**
`view_item`, `add_to_cart`, `begin_checkout`, `purchase`

**Importantes (semana 1):**
`view_item_list`, `select_item`, `remove_from_cart`, `view_cart`, `add_shipping_info`, `add_payment_info`, `search`

**Deseables (semana 2-3):**
`view_promotion`, `select_promotion`, `add_to_wishlist`, `refund`, eventos custom

**No implementar hasta que lo anterior funcione al 100%.**
