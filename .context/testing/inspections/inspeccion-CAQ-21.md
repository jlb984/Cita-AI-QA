# Reporte de Inspección de Requisitos: Cancelación de un turno por el profesional

**Historia:** CAQ-21
**Fecha:** 04/09/2026
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| CAQ-21-D1 | Contradice al sistema | La interfaz informó una cancelación aparente, pero la API mantuvo el turno `confirmed` y el slot siguió ocupado. | Hacer atómica la persistencia de `cancelled` y la liberación del slot; responder éxito solo después de ambas. |

## 2. Versión Corregida de la Historia

La versión vigente de `story.md` contiene 7 escenarios verificables e incorpora las decisiones aprobadas de Producto. El requisito quedó definido; no se convierte el comportamiento observado en criterio. La discrepancia de implementación permanece bloqueante hasta contar con nueva evidencia.

## 3. Valoración de Calidad

* **Veredicto:** Bloqueante
* **Riesgo:** Alto

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales\stories\STORY-CAQ-21-cancelacion-por-profesional\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 4 |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-21 y decisiones transversales aplicables |
| Comportamiento contrastado | **Observado** — producción, 02/09/2026. Evidencia: `.context/PBI/epics/EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-CAQ-21-cancelacion-por-profesional/evidence/2026-09-02-api-turno-permanece-confirmed.png` y `.context/PBI/epics/EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-CAQ-21-cancelacion-por-profesional/evidence/2026-09-02-api-slot-cancelado-no-liberado.png`. |

## Contradicciones detectadas

* El requisito exige persistir cancelled y liberar el slot antes de informar éxito; la evidencia del 02/09/2026 mostró el turno confirmed y el slot ocupado. La discrepancia queda como defecto bloqueante de implementación.

## Preguntas abiertas

* ¿Cuál es la causa técnica del falso éxito observado y llegó a intentarse el envío del correo de cancelación?
