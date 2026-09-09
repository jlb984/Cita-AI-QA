# Reporte de Inspección de Requisitos: Bloqueo de períodos puntuales

**Historia:** CAQ-13
**Fecha:** 09/09/2026 (reinspección con comportamiento observado; anterior: 04/09/2026)
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| 1 | Caso Borde | Las decisiones prohíben bloqueos con fin en el pasado, pero ningún escenario Gherkin lo cubre. | Se agrega el escenario 6 en la versión corregida (sale de las decisiones, no es hipótesis). |
| 2 | Caso Borde | El motivo es opcional pero ningún escenario fija un largo máximo ni el mensaje si se excede. | Agregar escenario o pregunta a PO. |

## 2. Versión Corregida de la Historia

Se agrega a los criterios del `story.md`:

### Escenario 6: Rechazar un bloqueo con fin en el pasado
**Given** que el profesional informa un fin anterior al momento actual
**When** intenta crear el bloqueo
**Then** el sistema rechaza la operación
**And** no altera la disponibilidad pública

El alta válida, la eliminación con confirmación (`¿Estás seguro de eliminar este bloqueo?`) y el estado `No hay bloqueos activos` quedaron verificados de punta a punta el 09/09/2026. Rango inválido, superpuestos, aviso sobre turnos confirmados y cruce de medianoche siguen sin recorrerse.

## 3. Valoración de Calidad

* **Veredicto:** Requiere cambios
* **Riesgo:** Medio

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos\stories\STORY-CAQ-13-bloqueo-de-periodos\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 2 |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13 y decisiones transversales aplicables |
| Prohibición de fin en el pasado | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13 |
| Alta y eliminación observadas | **Observado** — producción, 09/09/2026. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-13-bloqueo-de-periodos/evidence/2026-09-09-bloqueo-creado.png` |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Tope de motivo respondido por decisión delegada QA 09/09/2026 (250 caracteres, pendiente de ratificación PO) y trasladado al `story.md` (escenario 7). El texto exacto del mensaje sigue pendiente de observación.
