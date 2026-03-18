# 🏷️ Google Tag Manager

Guías de configuración avanzada, referencia de dataLayer y recursos prácticos para GTM.

## Contenido

### Guías

| Guía | Descripción |
|------|-------------|
| [Consent Mode v2](./guias/consent-mode-v2.md) | Configuración completa de Consent Mode v2 en GTM para cumplir con la normativa europea |
| [Ecommerce dataLayer](./guias/ecommerce-datalayer.md) | Implementación paso a paso del dataLayer de ecommerce GA4 |

### Referencia

| Recurso | Descripción |
|---------|-------------|
| [dataLayer Reference](./datalayer/datalayer-reference.md) | Referencia completa de todos los push de dataLayer para ecommerce GA4 |
| [Regex Cheatsheet](./regex-cheatsheet.md) | Patrones de regex más utilizados en GTM |

### Contenedores

| Recurso | Descripción |
|---------|-------------|
| [Contenedores exportados](./containers/) | Archivos JSON de contenedores GTM listos para importar |

## Filosofía de trabajo con GTM

1. **Nombra todo con convención clara.** Tags: `GA4 - Event - nombre_evento`. Triggers: `Trigger - descripción`. Variables: `DLV - nombre` (Data Layer Variable).
2. **Un tag por evento.** Evita meter lógica compleja en un solo tag. Es más fácil de debuggear y mantener.
3. **Usa carpetas.** Organiza tags, triggers y variables en carpetas por funcionalidad (GA4, Consent, Ads, etc.).
4. **Versiona siempre.** Antes de publicar, crea una versión con nombre descriptivo y notas de los cambios.
5. **Testea en Preview.** Nunca publiques sin pasar por el modo Preview y verificar en DebugView de GA4.
