# Reporte de Inspección de Requisitos: Configuración de disponibilidad semanal

**Historia:** CAQ-11
**Fecha:** 09/09/2026 (reinspección con comportamiento observado; anterior: 04/09/2026)
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| 1 | Contradice al sistema | Las decisiones aprueban `La hora de fin debe ser posterior a la hora de inicio.`; el sistema muestra `La hora de inicio debe ser anterior a la de fin en Martes`. | Confirmar cuál texto rige el release 1.1; no se elige aquí. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-11-configuracion-de-disponibilidad-semanal/evidence/2026-09-09-mensaje-rango-invalido.png` |
| 2 | Caso Borde | Ningún escenario define qué pasa con turnos existentes y slots públicos cuando se desactiva un día completo. | Agregar escenario o pregunta a PO. |
| 3 | Caso Borde | Ningún escenario define un máximo de intervalos por día ni el mensaje si se supera. | Agregar escenario o pregunta a PO. |

## 2. Versión Corregida de la Historia

Los 6 escenarios vigentes se conservan: el guardado válido y el rechazo de rango inválido quedaron verificados de punta a punta el 09/09/2026. Se agregan a *Preguntas abiertas* del `story.md` los defectos 2 y 3. El defecto 1 ya estaba registrado con las dos versiones en *Contradicciones detectadas*; no se modifica ninguna redacción sin decisión de negocio.

## 3. Valoración de Calidad

* **Veredicto:** Requiere cambios
* **Riesgo:** Medio

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos\stories\STORY-CAQ-11-configuracion-de-disponibilidad-semanal\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 2 |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-11 y decisiones transversales aplicables |
| Mensaje de rango inválido observado | **Observado** — producción, 09/09/2026. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-11-configuracion-de-disponibilidad-semanal/evidence/2026-09-09-mensaje-rango-invalido.png` |
| Guardado válido con persistencia confirmada | **Observado** — producción, 09/09/2026. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-11-configuracion-de-disponibilidad-semanal/evidence/2026-09-09-guardado-ok.png` |

## Contradicciones detectadas

* Texto aprobado vs texto mostrado para rango inválido (ver defecto 1). Sin resolver.

## Preguntas abiertas

* Día desactivado y tope de intervalos: respondidos por decisión delegada QA 09/09/2026 en el documento de decisiones (pendiente de ratificación PO) y trasladados al `story.md` (escenario 7).
* ¿Vale el texto observado con día incluido o el texto aprobado para el release 1.1?
