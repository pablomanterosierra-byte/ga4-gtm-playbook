# Dashboard Ecommerce — Estructura para Looker Studio

Estructura recomendada para un dashboard de ecommerce conectado a GA4 vía Looker Studio.

## Página 1: Overview (para dirección y stakeholders)

### Filtros globales
- Selector de fechas (con comparación activada)
- País / mercado (si aplica)
- Dispositivo (desktop / mobile / tablet)

### Fila 1 — KPIs principales (scorecards con comparación)

| KPI | Métrica GA4 | Por qué |
|-----|-------------|---------|
| Ingresos | `ecommerce purchases` → Revenue | El número más importante |
| Transacciones | `ecommerce purchases` → count | Volumen de ventas |
| Tasa de conversión | `sessions` con `purchase` / `sessions` totales | Eficiencia del sitio |
| AOV | Revenue / Transactions | Valor medio por pedido |
| Sesiones | `sessions` | Tráfico total |
| Revenue por sesión | Revenue / Sessions | Calidad del tráfico |

### Fila 2 — Tendencias

- **Gráfico de líneas:** Revenue diario (con comparación vs período anterior)
- **Gráfico de barras:** Transacciones por día de la semana (detectar patrones)

### Fila 3 — Desgloses

- **Tabla:** Top 10 productos por revenue (con transacciones, AOV, y cantidad)
- **Tabla:** Top 5 categorías por revenue
- **Gráfico de barras horizontal:** Revenue por canal de adquisición (organic, paid, direct, email, social)

## Página 2: Funnel de conversión (para producto/UX)

### Fila 1 — Funnel visual

| Paso | Evento GA4 | Métrica |
|------|-----------|---------|
| Ver producto | `view_item` | Usuarios |
| Añadir al carrito | `add_to_cart` | Usuarios |
| Iniciar checkout | `begin_checkout` | Usuarios |
| Info de envío | `add_shipping_info` | Usuarios |
| Info de pago | `add_payment_info` | Usuarios |
| Compra | `purchase` | Usuarios |

Mostrar el % de drop-off entre cada paso.

### Fila 2 — Tasas de conversión por paso

| Tasa | Fórmula |
|------|---------|
| View → Add to cart | `add_to_cart users / view_item users` |
| Add to cart → Checkout | `begin_checkout users / add_to_cart users` |
| Checkout → Purchase | `purchase users / begin_checkout users` |

### Fila 3 — Comparativa por dispositivo

Tabla con las tasas de conversión de cada paso, desglosado por desktop vs mobile. El drop-off móvil suele ser mayor en el checkout — aquí es donde se identifican oportunidades de optimización.

## Página 3: Adquisición (para marketing)

### Fila 1 — KPIs por canal

Tabla con estas columnas por canal de adquisición:

| Columna | Descripción |
|---------|-------------|
| Canal | Default channel grouping de GA4 |
| Sesiones | Volumen de tráfico |
| % Nuevos usuarios | Capacidad de captar tráfico nuevo |
| Tasa de conversión | Eficiencia del canal |
| Revenue | Ingresos generados |
| Revenue / sesión | Calidad del tráfico |

### Fila 2 — Tendencia por canal

Gráfico de líneas con los 3-4 canales principales (organic search, paid search, direct, email) mostrando revenue a lo largo del tiempo.

### Fila 3 — Detalle de campañas

Tabla filtrable por canal con:
- Campaña (utm_campaign)
- Medio (utm_medium)
- Sesiones
- Revenue
- Tasa de conversión
- ROAS (si tienes datos de coste)

## Métricas calculadas útiles en Looker Studio

| Métrica | Fórmula | Uso |
|---------|---------|-----|
| Tasa de conversión | `Ecommerce purchases / Sessions` | Eficiencia general |
| AOV | `Purchase revenue / Ecommerce purchases` | Valor medio de pedido |
| Revenue por sesión | `Purchase revenue / Sessions` | Calidad del tráfico |
| Tasa add-to-cart | `Add to carts / Sessions` | Interés en productos |
| Tasa de abandono de carrito | `1 - (Ecommerce purchases / Add to carts)` | Pérdida en el funnel |

## Tips de configuración en Looker Studio

1. **Usa "GA4 connector" nativo**, no conectores de terceros. Es gratuito y se actualiza automáticamente.
2. **Crea blended data sources** si necesitas combinar datos de GA4 con datos de costes (Google Ads, por ejemplo).
3. **Configura la caché de datos** a 12h para dashboards que no necesiten datos en tiempo real. Mejora mucho el rendimiento.
4. **Usa parámetros de informe** para crear filtros reutilizables (ej: selector de mercado que afecte a todas las páginas).
5. **Nombra las métricas calculadas** de forma clara. "Métrica 1" no le dice nada a nadie 6 meses después.
