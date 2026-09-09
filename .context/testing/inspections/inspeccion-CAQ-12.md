# Reporte de Inspección de Requisitos: Duración estándar de los turnos

**Historia:** CAQ-12
**Fecha:** 09/09/2026 (reinspección con comportamiento observado; anterior: 04/09/2026)
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| — | — | No se detectaron defectos nuevos. El catálogo observado (15, 30, 45, 60, 90, 120 con 30 seleccionado) coincide con las decisiones del release 1.1. El cambio de valor, los remanentes y los turnos existentes no se recorrieron (muta slots en producción) y siguen como cobertura pendiente, no como defecto. | Ninguna. |

## 2. Versión Corregida de la Historia

Sin cambios: los 5 escenarios vigentes cubren happy path y bordes (valor no positivo, remanente incompleto, fuera de catálogo, preservación de turnos). La contradicción histórica especificación-vs-catálogo ya está resuelta por las decisiones de Producto.

## 3. Valoración de Calidad

* **Veredicto:** Aprobado
* **Riesgo:** Medio

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos\stories\STORY-CAQ-12-duracion-estandar-de-turnos\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 2 |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-12 y decisiones transversales aplicables |
| Catálogo y valor seleccionado observados | **Observado** — producción, 09/09/2026. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-12-duracion-estandar-de-turnos/evidence/2026-09-09-duracion-cita.png` |

## Contradicciones detectadas

* La especificación histórica admitía cualquier entero positivo y la interfaz observada ofrecía un catálogo cerrado. Producto adopta 15, 30, 45, 60, 90 y 120 minutos para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
