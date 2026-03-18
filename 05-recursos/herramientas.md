# Herramientas útiles del ecosistema de analítica digital

Herramientas que uso o recomiendo para trabajar con GA4 y GTM de forma más eficiente.

## Extensiones de navegador

| Herramienta | Para qué | Enlace |
|-------------|----------|--------|
| **Google Tag Assistant** | Depurar GTM y GA4 directamente en el navegador. Ver qué tags se disparan y qué datos envían | [Chrome Web Store](https://chrome.google.com/webstore/detail/tag-assistant-legacy-by-g/kejbdjndbnbjgmefkgdddjlbokphdefk) |
| **Adswerve dataLayer Inspector+** | Visualizar el dataLayer en tiempo real de forma más clara que la consola | [Chrome Web Store](https://chrome.google.com/webstore/detail/datalayer-inspector%2B/bmdblncegkenkacieihfhpjfppoconhi) |
| **WASP Inspector** | Analizar las peticiones de analítica que salen del navegador (GA4, Facebook, etc.) | [Chrome Web Store](https://chrome.google.com/webstore/detail/wasp-inspector/niaoghengfohplclhbjnjheodgkejpih) |
| **GA4 Event Export** | Exportar eventos de GA4 DebugView para documentar | Buscar en Chrome Web Store |
| **EditThisCookie** | Ver y editar cookies del navegador. Útil para debuggear cookies de GA4 | [Chrome Web Store](https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg) |

## Herramientas de testing y validación

| Herramienta | Para qué |
|-------------|----------|
| **GTM Preview Mode** | Depurar el contenedor de GTM antes de publicar. Imprescindible. |
| **GA4 DebugView** | Ver eventos en tiempo real en GA4. Se activa con la extensión Tag Assistant o con el parámetro `debug_mode=true`. |
| **Google Analytics Debugger** | Extensión que activa el modo debug de GA4 automáticamente. |
| **regex101.com** | Testear expresiones regulares antes de usarlas en GTM. Selecciona "JavaScript" como flavor. |

## Herramientas de documentación y planificación

| Herramienta | Para qué |
|-------------|----------|
| **Google Sheets** | Tracking plans colaborativos. Más flexible que Markdown para documentos vivos que editan varias personas. |
| **Notion** | Documentación de proyectos de analítica. Bueno para combinar texto, tablas y bases de datos. |
| **Whimsical / Miro** | Diagramar flujos de datos: de dónde sale cada dato, por dónde pasa (dataLayer → GTM → GA4), a dónde llega. |

## Herramientas de análisis y reporting

| Herramienta | Para qué |
|-------------|----------|
| **Looker Studio** | Dashboards conectados a GA4. Gratuito. Ver la [sección de dashboards](../04-dashboards/). |
| **BigQuery** | Análisis SQL sobre datos raw de GA4. Gratuito hasta 1TB de consultas/mes y 10GB de almacenamiento. |
| **Google Colab** | Notebooks de Python para análisis avanzado de datos exportados de GA4/BigQuery. Gratuito. |

## Herramientas de auditoría

| Herramienta | Para qué |
|-------------|----------|
| **Google Tag Manager Audit (Simo Ahava)** | Auditar contenedores de GTM: tags sin trigger, variables no usadas, etc. |
| **GA4 Setup Assistant** | Dentro de GA4 (Admin), chequea que la configuración básica esté completa. |
| **Screaming Frog** | Rastrear todo el sitio web para verificar que GTM está instalado en todas las páginas y detectar problemas de implementación. |
