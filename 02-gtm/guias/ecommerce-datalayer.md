# Ecommerce dataLayer — Guía de implementación para GA4

## ¿Qué es el dataLayer?

El dataLayer es un array de JavaScript (`window.dataLayer`) que actúa como puente entre tu sitio web y Google Tag Manager. Cada vez que ocurre algo relevante en la web, se hace un `push` al dataLayer con los datos del evento, y GTM los recoge para enviarlos a GA4 (u otras herramientas)..

```javascript
// Así se ve el dataLayer en consola
window.dataLayer = [
  { event: "gtm.js", ... },               // GTM se cargó
  { event: "view_item", ecommerce: {...} } // El usuario vio un producto
];
```

## Regla de oro: limpiar antes de cada push

Siempre ejecuta `dataLayer.push({ ecommerce: null })` antes de un push de ecommerce. Esto limpia el objeto `ecommerce` del push anterior.

Si no lo haces, los datos del evento anterior pueden "contaminar" el siguiente. Es la fuente de errores más común y más difícil de diagnosticar.

```javascript
// ✅ Correcto: limpiar primero
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({ event: "view_item", ecommerce: { ... } });

// ❌ Incorrecto: sin limpiar
window.dataLayer.push({ event: "view_item", ecommerce: { ... } });
```

## Eventos de ecommerce GA4: referencia completa

### view_item_list

Se dispara cuando se muestra un listado de productos al usuario.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "view_item_list",
  ecommerce: {
    item_list_id: "categoria_camisetas",
    item_list_name: "Camisetas",
    items: [
      {
        item_id: "SKU-001",
        item_name: "Camiseta Básica Blanca",
        item_brand: "MarcaX",
        item_category: "Ropa",
        item_category2: "Camisetas",
        price: 19.95,
        index: 0
      },
      {
        item_id: "SKU-002",
        item_name: "Camiseta Estampada Negra",
        item_brand: "MarcaX",
        item_category: "Ropa",
        item_category2: "Camisetas",
        price: 24.95,
        index: 1
      }
    ]
  }
});
```

> **Rendimiento:** Si el listado tiene muchos productos (+50), considera enviar solo los visibles en pantalla. No es necesario enviar todos los productos de la página en un solo push.

### select_item

Se dispara cuando el usuario hace clic en un producto del listado.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "select_item",
  ecommerce: {
    item_list_id: "categoria_camisetas",
    item_list_name: "Camisetas",
    items: [{
      item_id: "SKU-001",
      item_name: "Camiseta Básica Blanca",
      item_brand: "MarcaX",
      item_category: "Ropa",
      item_category2: "Camisetas",
      price: 19.95,
      index: 0
    }]
  }
});
```

### view_item

Se dispara al cargar la página de detalle de producto (PDP).

```javascript
window.dataLayer.push({ ecommerce: null });
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

Se dispara cuando el usuario hace clic en "Añadir al carrito".

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

### remove_from_cart

Se dispara cuando el usuario elimina un producto del carrito.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "remove_from_cart",
  ecommerce: {
    currency: "EUR",
    value: 29.95,
    items: [{
      item_id: "SKU-12345",
      item_name: "Camiseta Básica Algodón",
      price: 29.95,
      quantity: 1
    }]
  }
});
```

### view_cart

Se dispara al abrir o cargar la página del carrito.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "view_cart",
  ecommerce: {
    currency: "EUR",
    value: 79.85,
    items: [
      {
        item_id: "SKU-12345",
        item_name: "Camiseta Básica Algodón",
        price: 29.95,
        quantity: 2
      },
      {
        item_id: "SKU-67890",
        item_name: "Pantalón Chino Slim",
        price: 49.95,
        quantity: 1
      }
    ]
  }
});
```

### begin_checkout

Se dispara cuando el usuario inicia el proceso de checkout.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "begin_checkout",
  ecommerce: {
    currency: "EUR",
    value: 79.85,
    coupon: "VERANO20",
    items: [
      {
        item_id: "SKU-12345",
        item_name: "Camiseta Básica Algodón",
        price: 29.95,
        quantity: 2
      },
      {
        item_id: "SKU-67890",
        item_name: "Pantalón Chino Slim",
        price: 49.95,
        quantity: 1
      }
    ]
  }
});
```

### add_shipping_info

Se dispara cuando el usuario completa los datos de envío.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "add_shipping_info",
  ecommerce: {
    currency: "EUR",
    value: 79.85,
    coupon: "VERANO20",
    shipping_tier: "Express 24h",
    items: [/* mismos items del checkout */]
  }
});
```

### add_payment_info

Se dispara cuando el usuario introduce los datos de pago.

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "add_payment_info",
  ecommerce: {
    currency: "EUR",
    value: 79.85,
    coupon: "VERANO20",
    payment_type: "Tarjeta de crédito",
    items: [/* mismos items del checkout */]
  }
});
```

### purchase

Se dispara en la página de confirmación de compra (thank you page).

```javascript
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "purchase",
  ecommerce: {
    transaction_id: "TXN-2026-00147",
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

### refund

Se dispara cuando se procesa un reembolso (normalmente desde backend).

```javascript
// Reembolso completo
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "refund",
  ecommerce: {
    transaction_id: "TXN-2026-00147"
  }
});

// Reembolso parcial (solo un producto)
window.dataLayer.push({ ecommerce: null });
window.dataLayer.push({
  event: "refund",
  ecommerce: {
    transaction_id: "TXN-2026-00147",
    items: [{
      item_id: "SKU-12345",
      quantity: 1
    }]
  }
});
```

## Configuración en GTM

Para cada evento de ecommerce, necesitas en GTM:

1. **Un trigger** de tipo "Custom Event" con el nombre del evento (ej: `add_to_cart`).
2. **Un tag** de tipo "GA4 Event" vinculado a tu tag de configuración GA4.
3. **Variables de Data Layer** para extraer los parámetros de `ecommerce`.

### Variables clave

| Variable en GTM | Tipo | Ruta del Data Layer |
|----------------|------|---------------------|
| DLV - ecommerce.items | Data Layer Variable | `ecommerce.items` |
| DLV - ecommerce.value | Data Layer Variable | `ecommerce.value` |
| DLV - ecommerce.currency | Data Layer Variable | `ecommerce.currency` |
| DLV - ecommerce.transaction_id | Data Layer Variable | `ecommerce.transaction_id` |
| DLV - ecommerce.shipping | Data Layer Variable | `ecommerce.shipping` |
| DLV - ecommerce.tax | Data Layer Variable | `ecommerce.tax` |
| DLV - ecommerce.coupon | Data Layer Variable | `ecommerce.coupon` |

> **Tip:** Usa el prefijo `DLV -` para todas las Data Layer Variables. Así las identificas rápidamente entre todas las variables del contenedor.

## Errores frecuentes

| Error | Síntoma | Solución |
|-------|---------|----------|
| No limpiar `ecommerce` antes del push | Eventos con datos del evento anterior mezclados | Añadir `dataLayer.push({ ecommerce: null })` antes de cada push |
| `value` como string | El valor de la transacción aparece como 0 en GA4 | Asegurar que `value` es tipo `number`: `29.95` no `"29.95"` |
| `currency` ausente | GA4 ignora el `value` si no hay `currency` | Incluir siempre `currency: "EUR"` junto con `value` |
| `transaction_id` duplicado | Transacciones duplicadas en los informes | Generar un ID único por pedido desde backend |
| Items vacíos | El evento se dispara pero sin datos de producto | Verificar que el array `items` se popula correctamente |
