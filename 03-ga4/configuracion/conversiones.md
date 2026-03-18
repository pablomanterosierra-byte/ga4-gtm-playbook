## Qué son los Key Events

En GA4, un Key Event (antes llamado "Conversión") es un evento que marcas como especialmente importante para tu negocio. Al marcarlo, GA4 lo destaca en los informes, lo incluye en los modelos de atribución, y lo hace disponible para exportar a Google Ads como conversión de campaña.

> **Nota de nomenclatura:** Google renombró "Conversiones" a "Key Events" en 2024. En la interfaz de GA4 ahora verás "Key Events". En Google Ads sigue siendo "Conversiones". Mismo concepto, diferente nombre según la herramienta.

## Criterios para decidir qué marcar como Key Event

No todo lo que es "importante" debería ser un Key Event. Si marcas demasiados, diluyes el análisis. Mi criterio:

**Marcar como Key Event si:**
- Representa un momento de valor claro para el negocio (compra, lead, suscripción).
- Necesitas usarlo como conversión en Google Ads.
- Es un paso clave del funnel que quieres ver en los modelos de atribución.

**NO marcar como Key Event si:**
- Es un evento de interacción genérico (scroll, clic en menú, ver FAQ).
- Tiene un volumen tan alto que no es accionable (page_view, session_start).
- No tiene impacto directo en los objetivos de negocio.

## Key Events recomendados para ecommerce

| Key Event | Evento base | Valor monetario | Justificación |
|-----------|-------------|-----------------|---------------|
| **Compra** | `purchase` | Sí — dinámico (`value`) | El evento más importante. Es el objetivo final. |
| **Inicio de checkout** | `begin_checkout` | No | Paso clave del funnel. Permite medir el drop-off entre carrito y checkout. |
| **Añadir al carrito** | `add_to_cart` | No | Indicador de intención de compra. Útil para remarketing. |
| **Suscripción newsletter** | `newsletter_signup` | No | Micro-conversión. Captura de lead para email marketing. |

> **Límite:** GA4 permite un máximo de 30 Key Events. En ecommerce, 4-6 es un buen número.

## Cómo configurar un Key Event

### En la interfaz de GA4

1. Ve a **Admin → Key Events** (antes "Conversions").
2. Haz clic en **"New key event"**.
3. Introduce el nombre exacto del evento (ej: `purchase`).
4. Actívalo.

Si el evento ya se ha disparado alguna vez, aparecerá en la lista de eventos (Admin → Events) y puedes marcarlo directamente desde ahí con el toggle.

### Valor monetario

Para `purchase`, GA4 toma automáticamente el `value` y `currency` del evento. No necesitas configuración adicional.

Para otros Key Events donde quieras asignar un valor fijo (ej: `newsletter_signup` = 5€), puedes:
1. Ir a Admin → Key Events → clic en el evento.
2. Activar "Default value" y asignar un importe.

O enviarlo dinámicamente desde el dataLayer:

```javascript
window.dataLayer.push({
  event: "newsletter_signup",
  value: 5.00,
  currency: "EUR"
});
```

## Atribución

Los Key Events son los que alimentan los modelos de atribución de GA4. Esto significa que GA4 analiza qué canales y campañas contribuyeron a que el usuario completara ese Key Event.

GA4 usa el modelo **data-driven attribution** por defecto, que distribuye el crédito entre los puntos de contacto según su contribución real (basado en machine learning).

Si solo tienes eventos genéricos como Key Events, la atribución no te va a contar nada útil. Por eso es importante ser selectivo.

## Enviar Key Events a Google Ads

Para usar un Key Event de GA4 como conversión en Google Ads:

1. Vincula GA4 con Google Ads (Admin → Google Ads Linking).
2. En Google Ads, ve a **Herramientas → Conversiones → Importar**.
3. Selecciona **Google Analytics 4** y elige los Key Events que quieras importar.

> **Importante:** Para campañas de Smart Bidding (Target ROAS, Maximize Revenue), el Key Event `purchase` con valor dinámico es esencial. Sin valor monetario, las estrategias de puja automática no pueden optimizar por revenue.

## Errores frecuentes

| Error | Consecuencia | Solución |
|-------|-------------|----------|
| Marcar `page_view` como Key Event | Todos los canales tienen "conversiones", la atribución no sirve | Desmarcar y usar solo eventos con valor de negocio |
| No incluir `currency` con `value` | GA4 ignora el valor | Enviar siempre `currency: "EUR"` junto con `value` |
| Duplicar `purchase` | Revenue inflado | Verificar que `transaction_id` es único |
| Marcar +15 Key Events | Los informes se vuelven confusos | Limitar a 4-6 Key Events clave |
