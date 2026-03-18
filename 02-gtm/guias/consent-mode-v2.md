# Consent Mode v2 — Guía de configuración en GTM

## ¿Qué es Consent Mode?

Consent Mode es el mecanismo de Google para adaptar el comportamiento de sus etiquetas (GA4, Google Ads, Floodlight) al consentimiento del usuario. En lugar de bloquear o permitir las etiquetas por completo, Consent Mode les indica si tienen o no permiso para leer/escribir cookies, y las etiquetas ajustan su comportamiento en consecuencia.

## ¿Por qué es obligatorio?

Desde marzo de 2024, Google exige Consent Mode v2 para poder seguir usando funcionalidades de audiencias y remarketing en el Espacio Económico Europeo (EEE). Sin Consent Mode, las audiencias de GA4 y Google Ads dejan de funcionar en Europa.

## Los dos modos: Basic vs Advanced

| Aspecto | Basic Mode | Advanced Mode |
|---------|------------|---------------|
| **Comportamiento sin consentimiento** | Las etiquetas NO se disparan | Las etiquetas se disparan pero sin cookies (pings sin datos personales) |
| **Datos recogidos sin consentimiento** | Ninguno | Datos agregados y anónimos (modelización) |
| **Modelización de conversiones** | No disponible | Disponible (Google usa los pings para modelar) |
| **Implementación** | Más sencilla | Requiere configuración adicional |
| **Recomendado para** | Cumplimiento estricto mínimo | Maximizar datos respetando privacidad |

> **Mi recomendación:** Advanced Mode siempre que sea posible. La modelización de GA4 necesita esos pings sin cookies para poder rellenar los huecos de datos de los usuarios que no dan consentimiento.

## Los consent types

Consent Mode v2 trabaja con estos tipos de consentimiento:

| Tipo | Qué controla |
|------|-------------|
| `ad_storage` | Cookies de publicidad (Google Ads, remarketing) |
| `ad_user_data` | Envío de datos de usuario a Google para publicidad **(nuevo en v2)** |
| `ad_personalization` | Personalización de anuncios **(nuevo en v2)** |
| `analytics_storage` | Cookies de analítica (GA4) |
| `functionality_storage` | Cookies funcionales (idioma, preferencias) |
| `personalization_storage` | Cookies de personalización (recomendaciones) |
| `security_storage` | Cookies de seguridad (autenticación, fraude) |

Los dos nuevos en v2 (`ad_user_data` y `ad_personalization`) son el motivo por el que hay que migrar de v1 a v2.

## Configuración paso a paso en GTM

### Paso 1: Configurar los valores por defecto

En GTM, ve a **Administrar → Configuración del contenedor → Configuración adicional** y habilita "Enable consent overview".

Luego crea un tag de tipo **"Consent Initialization - All Pages"** que establezca los valores por defecto. Este tag usa el trigger especial "Consent Initialization" que se dispara antes de todo lo demás.

```javascript
// Valores por defecto: todo denegado hasta que el usuario acepte
gtag('consent', 'default', {
  'ad_storage': 'denied',
  'ad_user_data': 'denied',
  'ad_personalization': 'denied',
  'analytics_storage': 'denied',
  'functionality_storage': 'denied',
  'personalization_storage': 'denied',
  'security_storage': 'granted',
  'wait_for_update': 500
});
```

> **`wait_for_update: 500`** le dice a GTM que espere hasta 500ms a que el CMP (banner de cookies) actualice el consentimiento antes de disparar las etiquetas. Ajusta este valor según la velocidad de carga de tu CMP.

### Paso 2: Integrar tu CMP (Consent Management Platform)

La mayoría de CMPs del mercado (Cookiebot, OneTrust, CookieYes, Usercentrics, etc.) ya tienen integración nativa con GTM y Consent Mode v2.

Ejemplo con **Cookiebot**:
1. En GTM, añade la etiqueta de Cookiebot desde la galería de plantillas.
2. Configura el trigger como "Consent Initialization - All Pages".
3. Cookiebot se encargará automáticamente de llamar a `gtag('consent', 'update', {...})` cuando el usuario interactúe con el banner.

Ejemplo con un **CMP genérico** (si necesitas hacerlo manual):

```javascript
// Cuando el usuario acepta (tu CMP debe ejecutar esto)
gtag('consent', 'update', {
  'ad_storage': 'granted',
  'ad_user_data': 'granted',
  'ad_personalization': 'granted',
  'analytics_storage': 'granted'
});
```

### Paso 3: Configurar los consent settings en cada tag

En cada etiqueta de GTM, puedes ver la sección "Consent Settings" (necesitas tener el overview habilitado).

| Tipo de tag | Consent requerido |
|-------------|-------------------|
| GA4 Configuration | `analytics_storage` |
| GA4 Event | `analytics_storage` |
| Google Ads Conversion | `ad_storage`, `ad_user_data` |
| Google Ads Remarketing | `ad_storage`, `ad_personalization` |
| Floodlight | `ad_storage`, `ad_user_data` |

GTM configura esto automáticamente para los tags de Google, pero verifica que las etiquetas de terceros también tengan los consent settings correctos.

### Paso 4: Configurar el modo (Basic o Advanced)

**Para Basic Mode:**
- En cada tag de Google (GA4, Ads), en Consent Settings, selecciona "No additional consent required" = desactivado. Esto hace que el tag NO se dispare sin consentimiento.

**Para Advanced Mode:**
- Deja las etiquetas en su configuración por defecto. GTM se encargará de disparar pings sin cookies cuando no haya consentimiento, y pings normales cuando lo haya.

## Cómo verificar que funciona

### 1. GTM Preview Mode
- Abre el modo Preview de GTM.
- Verifica que el tag "Consent Initialization" se dispara primero.
- Interactúa con el banner de cookies.
- Comprueba que los consent states cambian de `denied` a `granted` en la pestaña "Consent" del debugger.

### 2. GA4 DebugView
- Ve a GA4 → Admin → DebugView.
- Con consentimiento denegado, deberías ver eventos con un icono de escudo (datos modelados en Advanced Mode) o no ver eventos (Basic Mode).
- Tras aceptar cookies, los eventos deberían aparecer normalmente.

### 3. Network tab del navegador
- Abre DevTools → Network.
- Filtra por `google-analytics.com` o `googletagmanager.com`.
- Con consentimiento denegado en Advanced Mode, verás peticiones con `gcs=G100` (denied) o similar.
- Con consentimiento granted, verás `gcs=G111`.

## Impacto en los datos de GA4

Con Consent Mode activado, es normal ver:

- **Menos usuarios y sesiones** en los informes (los usuarios que rechazan cookies no se trackean con cookies, solo se modelan parcialmente).
- **Indicador de "datos modelados"** en GA4 (icono de triángulo). Esto es bueno: significa que la modelización está funcionando.
- **Diferencias entre Basic y Advanced:** Con Basic puedes perder un 30-60% de datos. Con Advanced, la modelización recupera parte de esos datos (Google estima que puede recuperar un 70-80% según el volumen de tráfico).

## Checklist de implementación

- [ ] Consent Initialization tag configurado con todos los consent types en `denied`
- [ ] CMP integrado y disparando `gtag('consent', 'update', ...)` correctamente
- [ ] `ad_user_data` y `ad_personalization` incluidos (requisito v2)
- [ ] Consent settings verificados en cada tag de GTM
- [ ] Modo (Basic/Advanced) elegido y configurado
- [ ] Testing con GTM Preview: consent states cambian correctamente
- [ ] Testing con GA4 DebugView: eventos se disparan según consentimiento
- [ ] Testing con Network tab: parámetros `gcs` correctos
- [ ] Documentado en el tracking plan del proyecto
