# 📊 Dashboards

Estructuras de dashboards y métricas clave para reporting de ecommerce con Looker Studio (Google Data Studio).

## Filosofía de reporting

Un buen dashboard no es el que tiene más datos, sino el que provoca acción. Antes de construir un dashboard, hazte estas preguntas:

1. **¿Quién lo va a usar?** No es lo mismo un dashboard para el CEO que para el equipo de marketing.
2. **¿Qué decisión tiene que tomar esa persona?** El dashboard debe responder a preguntas concretas.
3. **¿Con qué frecuencia lo va a consultar?** Diario, semanal, mensual — esto define el nivel de detalle.

## Contenido

| Dashboard | Descripción |
|-----------|-------------|
| [Ecommerce Overview](./looker-studio/plantilla-ecommerce.md) | Estructura y métricas del dashboard principal de ecommerce |

## Principios de diseño de dashboards

### 1. Estructura piramidal

Organiza la información de arriba a abajo, de lo general a lo específico:

- **Fila 1:** KPIs principales (scorecards) — lo primero que se ve.
- **Fila 2:** Tendencias temporales (gráficas de línea/barras) — contexto.
- **Fila 3:** Desgloses (tablas, gráficas por dimensión) — detalle.

### 2. Máximo 6-8 KPIs por página

Si necesitas más, crea páginas adicionales. Un dashboard con 20 métricas en una sola vista no comunica nada.

### 3. Contexto siempre

Un número sin contexto no sirve. Incluye siempre:
- **Período de comparación** (vs. período anterior, vs. mismo período año anterior).
- **Objetivos / targets** si existen.
- **Tendencia** (la dirección importa más que el número absoluto).

### 4. Un dashboard, una audiencia

No intentes hacer un dashboard universal. Haz uno para cada público:
- **Dirección:** Revenue, AOV, tasa de conversión, top productos. Vista mensual.
- **Marketing:** Revenue por canal, ROAS, coste por adquisición, campañas. Vista semanal.
- **Producto/UX:** Funnel de checkout, tasa de abandono, búsquedas sin resultado. Vista semanal.
