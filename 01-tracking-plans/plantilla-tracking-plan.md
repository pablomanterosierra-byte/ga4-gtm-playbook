# Plantilla de Tracking Plan — [Nombre del Proyecto]

> Copia este archivo, renómbralo, y rellena cada sección con los datos de tu proyecto.

## 1. Contexto de negocio

| Campo | Detalle |
|-------|---------|
| **Tipo de negocio** | [Ecommerce / SaaS / Lead gen / Media / ...] |
| **Plataforma** | [Web / App / Ambas] |
| **Herramientas** | [GA4 + GTM / GA4 + GTM Server-Side / ...] |
| **Objetivo principal** | [¿Qué quiere conseguir el negocio?] |
| **Fecha** | [YYYY-MM-DD] |
| **Autor** | [Tu nombre] |

## 2. KPIs principales

| KPI | Descripción | Fórmula / Fuente |
|-----|-------------|------------------|
| | | |
| | | |
| | | |

## 3. Mapa de eventos

### [Categoría 1: ej. Navegación]

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| | | | | |

### [Categoría 2: ej. Conversión]

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| | | | | |

### [Categoría 3: ej. Interacción]

| Evento | Trigger | Parámetros | Página | Prioridad |
|--------|---------|------------|--------|-----------|
| | | | | |

> **Tip:** Agrupa los eventos por categoría lógica (navegación, conversión, interacción, etc.), no por página. Un mismo evento puede dispararse en varias páginas.

## 4. Estructura de parámetros

| Parámetro | Tipo | Obligatorio | Ejemplo | Descripción |
|-----------|------|-------------|---------|-------------|
| | | | | |

## 5. Ejemplos de dataLayer push

```javascript
// Ejemplo de evento
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: "nombre_del_evento",
  // parámetros del evento
});
```

## 6. Dimensiones personalizadas

| Dimensión | Scope | Parámetro del evento | Uso |
|-----------|-------|---------------------|-----|
| | | | |

## 7. Key Events (Conversiones)

| Key Event | Evento base | Condiciones | Valor |
|-----------|-------------|-------------|-------|
| | | | |

## 8. Audiencias sugeridas

| Audiencia | Definición | Uso |
|-----------|------------|-----|
| | | |

## 9. Notas de implementación

- [ ] [Nota técnica 1]
- [ ] [Nota técnica 2]

## 10. QA Checklist

- [ ] Todos los eventos se disparan correctamente
- [ ] Los parámetros tienen el tipo de dato correcto
- [ ] Los Key Events están configurados en GA4
- [ ] Las dimensiones personalizadas están registradas
- [ ] No hay eventos duplicados
- [ ] Consent Mode funciona correctamente
