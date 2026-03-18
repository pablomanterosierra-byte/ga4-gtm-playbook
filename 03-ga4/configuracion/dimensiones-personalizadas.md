# Dimensiones y métricas personalizadas en GA4

## Qué son y para qué sirven

Las dimensiones personalizadas (custom dimensions) te permiten enviar datos adicionales a GA4 que no están cubiertos por los parámetros estándar. Son la forma de adaptar GA4 a la realidad de tu negocio.

Por ejemplo, GA4 sabe qué producto compró un usuario, pero no sabe si ese usuario era un cliente nuevo o recurrente según tu lógica de negocio. Para eso necesitas una custom dimension.

## Tipos de scope

| Scope | Qué describe | Se define en | Ejemplo |
|-------|-------------|-------------|---------|
| **Event** | Una acción concreta | Un evento específico | `page_type`, `filter_type`, `item_stock_status` |
| **User** | Una característica del usuario | User properties | `user_type`, `customer_lifetime_value_tier` |

> **Session scope desapareció en GA4.** Si necesitas algo a nivel de sesión (como la fuente de la visita), usa el scope de evento y analiza por sesión en los informes.

## Cómo crear una custom dimension

### Paso 1: Enviar el parámetro desde GTM (o dataLayer)

El parámetro debe estar presente en el evento que lo envía. Puede venir del dataLayer o configurarse directamente en el tag de GTM.

Ejemplo — enviar `page_type` en todos los `page_view`:

```javascript
// En el dataLayer de cada página
window.dataLayer.push({
  event: "page_view_custom",
  page_type: "PDP"  // PLP, PDP, checkout, home, etc.
});
```

O directamente en el tag de GA4 en GTM: añade `page_type` como parámetro del evento con una variable que lo calcule.

### Paso 2: Registrar la dimensión en GA4

1. Ve a **Admin → Custom definitions → Create custom dimension**.
2. Rellena:
   - **Dimension name:** Nombre descriptivo (ej: "Tipo de página")
   - **Scope:** Event o User
   - **Event parameter / User property:** El nombre exacto del parámetro (ej: `page_type`)
3. Guarda.

> **Importante:** Si no registras el parámetro como custom dimension, GA4 lo recibe pero no puedes usarlo en informes ni Explorations. El dato está en BigQuery, pero no en la interfaz.

## Custom dimensions recomendadas para ecommerce

### Event-scoped

| Nombre en GA4 | Parámetro | Descripción | Cuándo enviar |
|---------------|-----------|-------------|---------------|
| Tipo de página | `page_type` | Clasifica la página: home, PLP, PDP, cart, checkout, thankyou, other | Cada page_view |
| Estado de stock | `item_stock_status` | in_stock, low_stock, out_of_stock | `view_item` |
| Tipo de filtro | `filter_type` | color, talla, precio, marca | `filter_applied` (custom event) |
| Valor del filtro | `filter_value` | El valor específico: "azul", "M", "0-50€" | `filter_applied` |
| Método de newsletter | `newsletter_method` | footer, popup, checkout, blog | `newsletter_signup` |
| Tipo de búsqueda | `search_type` | Con resultados, sin resultados | `search` |

### User-scoped

| Nombre en GA4 | User property | Descripción | Cuándo setear |
|---------------|--------------|-------------|---------------|
| Tipo de cliente | `customer_type` | nuevo, recurrente, vip | Al cargar la página (si hay sesión de usuario) |
| Estado de login | `login_status` | logged_in, guest | Al cargar la página |
| Segmento de valor | `value_segment` | alto, medio, bajo (basado en histórico de compras) | Al cargar la página (si hay sesión) |

Para enviar user properties desde GTM, usa la sección "User Properties" del tag de configuración de GA4, o con dataLayer:

```javascript
// User property via gtag (si no usas GTM)
gtag('set', 'user_properties', {
  customer_type: 'recurrente',
  login_status: 'logged_in'
});

// User property via dataLayer (para GTM)
window.dataLayer.push({
  event: "user_data_ready",
  user_properties: {
    customer_type: "recurrente",
    login_status: "logged_in"
  }
});
```

## Custom metrics

Además de dimensiones, puedes crear métricas personalizadas. Son útiles cuando necesitas un valor numérico que GA4 no recoge por defecto.

| Métrica | Parámetro | Unidad | Uso |
|---------|-----------|--------|-----|
| Productos en carrito | `cart_item_count` | Estándar | Saber cuántos productos tiene el carrito al hacer checkout |
| Descuento aplicado | `discount_amount` | Moneda | Medir el impacto de los descuentos en revenue |

## Errores frecuentes

| Error | Consecuencia | Solución |
|-------|-------------|----------|
| No registrar el parámetro como custom dimension en GA4 | El dato llega pero no es visible en informes | Registrar en Admin → Custom definitions |
| Enviar el parámetro con nombre inconsistente | `pageType` en unas páginas, `page_type` en otras | Estandarizar el nombre en el tracking plan |
| Superar el límite de 50 event-scoped dimensions | No se pueden crear más | Planificar con el tracking plan y priorizar |
| Enviar datos personales (PII) como dimensiones | Violación de TOS de Google | Nunca enviar email, nombre, teléfono, IP como custom dimension |

## Checklist

- [ ] Cada custom dimension está documentada en el tracking plan
- [ ] El parámetro se envía correctamente desde GTM (verificar en Preview)
- [ ] La dimensión está registrada en GA4 (Admin → Custom definitions)
- [ ] El dato aparece en DebugView con el valor correcto
- [ ] No se envía PII (datos personales identificables)
