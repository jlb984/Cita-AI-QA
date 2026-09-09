# Reporte de Inspección de Requisitos: Registro manual de un turno

**Historia:** CAQ-14
**Fecha:** 09/09/2026 (reinspección con comportamiento observado; anterior: 04/09/2026)
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| 1 | Contradice al sistema | Los 6 escenarios describen un flujo del panel que no existe: `/dashboard`, `/dashboard/availability` y `/dashboard/clients` no exponen alta manual de turnos. Los criterios son aspiraciones, no descripciones; nada de esto se puede probar. | Decidir con negocio: construir el flujo o dar de baja la historia. No se reescribe Gherkin sobre algo inexistente. |
| 2 | Agrega regla que el PRD no menciona | El registro manual no figura en `.context/architecture/prd.md` · Feature 2 (lista disponibilidad, duración, bloqueos, consulta y cancelación). La regla llega del hilo de alcance y soporte, nunca fue contrastada contra el PRD. | Confirmar con negocio si el registro manual es alcance vigente. |

## 2. Versión Corregida de la Historia

No hay versión corregida posible: corregir redacción sobre un flujo inexistente sería inventar cobertura. Se actualizan en el `story.md` el veredicto (`No encontrada`, ya registrado en la exploración), las contradicciones (defectos 1 y 2 con las dos versiones) y las preguntas abiertas. Todo lo que exige decisión de negocio queda marcado y sin resolver.

## 3. Valoración de Calidad

* **Veredicto:** Bloqueante
* **Riesgo:** Alto

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos\stories\STORY-CAQ-14-registro-manual-de-turno\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 2 (no lista registro manual) |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-14 y decisiones transversales aplicables |
| Ausencia del flujo en las rutas del panel | **Observado** — producción, 09/09/2026. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-14-registro-manual-de-turno/evidence/2026-09-09-clientes-sin-alta-turno.png` |

## Contradicciones detectadas

* Decisiones vs panel: flujo descripto contra panel sin esa acción (ver defecto 1).
* Hilo de alcance vs PRD: registro manual como alcance contra PRD que no lo lista (ver defecto 2).

## Preguntas abiertas

* ¿Dónde está el registro manual de turnos, o es funcionalidad no construida?
* ¿El registro manual es alcance vigente del producto aunque el PRD no lo liste?
