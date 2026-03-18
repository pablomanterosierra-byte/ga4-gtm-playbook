# Glosario de Analítica Digital — GA4 y GTM

Términos clave explicados de forma práctica, orientados a ecommerce.

## A

**Active Users:** En GA4, es la métrica principal de "usuarios". Cuenta usuarios que tuvieron una sesión con engagement (más de 10 segundos, o un evento de conversión, o 2+ page views). Es diferente al "Total Users" que cuenta a todos.

**Atribución:** El proceso de asignar crédito a los canales de marketing que contribuyeron a una conversión. GA4 usa Data-Driven Attribution por defecto, que distribuye el crédito según la contribución real de cada punto de contacto.

**Audiencia:** Grupo de usuarios definido por condiciones específicas (ej: "usuarios que añadieron al carrito pero no compraron en 7 días"). Se crean en GA4 y se pueden exportar a Google Ads para remarketing.

**AOV (Average Order Value):** Valor medio de pedido. Se calcula como revenue total / número de transacciones.

## B

**BigQuery:** Base de datos de Google Cloud donde puedes exportar los datos raw de GA4. Permite hacer análisis avanzados con SQL que no son posibles en la interfaz de GA4.

**Bounce Rate:** En GA4, es el opuesto de Engagement Rate. Un "bounce" es una sesión sin engagement (menos de 10s, sin conversión, sin 2+ page views). No confundir con el bounce rate de Universal Analytics.

## C

**Consent Mode:** Mecanismo de Google para adaptar las etiquetas al consentimiento del usuario. Ver la [guía completa](../02-gtm/guias/consent-mode-v2.md).

**Conversión:** Ver "Key Event".

**Cookie:** Pequeño archivo de texto que el navegador almacena. GA4 usa la cookie `_ga` para identificar usuarios y `_ga_<ID>` para la sesión. Consent Mode controla si estas cookies se pueden escribir.

**CMP (Consent Management Platform):** Herramienta que gestiona el banner de cookies y el consentimiento del usuario (ej: Cookiebot, OneTrust, CookieYes).

**Custom Dimension:** Dimensión personalizada que defines tú para enviar datos que GA4 no recoge por defecto. Ver la [guía completa](../03-ga4/configuracion/dimensiones-personalizadas.md).

## D

**Data Layer (dataLayer):** Array de JavaScript que actúa como puente entre el sitio web y GTM. Cada vez que ocurre algo relevante, se hace un `push` con los datos del evento. Ver la [referencia completa](../02-gtm/datalayer/datalayer-reference.md).

**Data Stream:** En GA4, una fuente de datos. Puede ser web, iOS o Android. Cada stream tiene su propio Measurement ID.

**DebugView:** Herramienta de GA4 (Admin → DebugView) que muestra los eventos en tiempo real mientras testeas. Esencial para verificar implementaciones.

**Default Channel Grouping:** Clasificación automática de GA4 del tráfico por canal: Organic Search, Paid Search, Direct, Email, Social, Referral, etc. Se basa en los parámetros UTM y las reglas de Google.

## E

**Engagement Rate:** Porcentaje de sesiones con engagement (más de 10 segundos, o conversión, o 2+ page views). Es el inverso de Bounce Rate.

**Enhanced Measurement:** Funcionalidad de GA4 que recoge automáticamente ciertos eventos (scroll, outbound clicks, file downloads, etc.) sin necesidad de configuración en GTM.

**Evento:** La unidad básica de medición en GA4. Todo es un evento: una página vista, un clic, una compra. Cada evento tiene un nombre y puede llevar parámetros.

**Explorations:** Herramienta de análisis avanzado de GA4 que permite crear informes personalizados: free-form, funnel, path, segment overlap, etc.

## F

**First-Party Data:** Datos que recoges directamente de tus usuarios (compras, navegación en tu web, suscripciones). Son los más valiosos y los menos afectados por restricciones de cookies.

**Funnel:** Secuencia de pasos que un usuario sigue hasta una conversión. En ecommerce: view_item → add_to_cart → begin_checkout → purchase. Cada paso donde se pierden usuarios es una oportunidad de optimización.

## G

**GA4 (Google Analytics 4):** La versión actual de Google Analytics, basada en un modelo de datos de eventos (no de sesiones como Universal Analytics).

**GTM (Google Tag Manager):** Herramienta para gestionar etiquetas (tags) de marketing y analítica sin modificar el código del sitio. Ver la [sección completa](../02-gtm/).

## K

**Key Event:** (Antes "Conversión" en GA4). Un evento que marcas como especialmente importante para tu negocio. Se destaca en informes y se usa en modelos de atribución. Ver la [guía completa](../03-ga4/configuracion/conversiones.md).

## L

**Looker Studio:** (Antes Google Data Studio). Herramienta gratuita de Google para crear dashboards y reportes conectados a GA4 y otras fuentes.

**LTV (Lifetime Value):** Valor total que un cliente genera a lo largo de su relación con tu negocio.

## M

**Measurement ID:** Identificador de un Data Stream de GA4. Formato: `G-XXXXXXXXXX`. Se usa en el tag de configuración de GA4 en GTM.

**Measurement Protocol:** API de GA4 para enviar eventos desde servidores (backend). Útil para eventos como refunds que no ocurren en el navegador.

## P

**PDP (Product Detail Page):** Página de detalle de producto. Donde se dispara `view_item`.

**PLP (Product Listing Page):** Página de listado de productos (categoría, búsqueda). Donde se dispara `view_item_list`.

**Preview Mode:** Modo de depuración de GTM que permite ver qué tags se disparan, qué triggers se activan, y qué valores tienen las variables, sin publicar los cambios.

## R

**ROAS (Return on Ad Spend):** Revenue generado por cada euro invertido en publicidad. Fórmula: Revenue / Coste publicitario.

## S

**Sesión:** Un período de actividad del usuario en tu sitio. En GA4, una sesión termina tras 30 minutos de inactividad o a medianoche. A diferencia de UA, un cambio de fuente de tráfico NO reinicia la sesión en GA4.

**Server-Side GTM:** Versión de GTM que procesa las etiquetas en un servidor en lugar del navegador del usuario. Mejora rendimiento, privacidad y control de datos. Requiere infraestructura en Google Cloud.

## T

**Tag:** En GTM, un fragmento de código que se ejecuta cuando se cumplen ciertas condiciones (triggers). Ejemplos: tag de configuración de GA4, tag de evento de GA4, pixel de Facebook.

**Tracking Plan:** Documento que define qué eventos se van a medir, con qué parámetros, en qué páginas, y por qué. Ver la [sección completa](../01-tracking-plans/).

**Trigger:** En GTM, la condición que activa un tag. Ejemplos: "Todas las páginas", "Custom Event: add_to_cart", "Click en elementos con clase .btn-comprar".

## U

**UTM:** Parámetros que se añaden a las URLs para identificar el origen del tráfico: `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`.

**User ID:** Identificador único del usuario que tú asignas (diferente del Client ID de GA4). Permite hacer cross-device tracking cuando el usuario está logueado.

## V

**Variable:** En GTM, un valor reutilizable que puede venir del dataLayer, de una cookie, de la URL, o de JavaScript personalizado. Se usan en tags y triggers.
