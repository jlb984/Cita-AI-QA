# Reporte de Inspección de Requisitos: Consulta de próximos turnos

**Historia:** CAQ-15
**Fecha:** 09/09/2026 (reinspección con comportamiento observado; anterior: 04/09/2026)
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| — | — | No se detectaron defectos de requisito. El estado vacío observado coincide literalmente con el escenario 2. La inconsistencia `Citas Hoy: 1` vs `No tienes citas próximas` es un hallazgo del sistema, no del texto: ya está registrado con ambas versiones en el `story.md` y no invalida ningún criterio. Los escenarios con datos quedan sin recorrer por falta de datos, no por defecto del requisito. | Ninguna. |

## 2. Versión Corregida de la Historia

Sin cambios: los 8 escenarios vigentes cubren happy path y bordes (vacío, aislamiento, orden, paginado, búsqueda, filtro, cancelados excluidos). El formato de zona horaria usado en cada fila no está fijado en ningún documento; se deja como pregunta abierta menor.

## 3. Valoración de Calidad

* **Veredicto:** Aprobado
* **Riesgo:** Medio

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos\stories\STORY-CAQ-15-consulta-de-proximos-turnos\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 2 |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-15 y decisiones transversales aplicables |
| Estado vacío y contadores observados | **Observado** — producción, 09/09/2026. Evidencia: `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-15-consulta-de-proximos-turnos/evidence/2026-09-09-dashboard-proximas-citas.png`, `../PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/stories/STORY-CAQ-15-consulta-de-proximos-turnos/evidence/2026-09-09-dashboard-contadores.png` |

## Contradicciones detectadas

* `Citas Hoy: 1` convive con `No tienes citas próximas` y `Próxima Cita --:--` en el mismo dashboard (hallazgo del sistema, registrado en el `story.md`; no es defecto del requisito).

## Preguntas abiertas

* Formato de zona respondido por 2.2 del documento de decisiones (identificador o abreviatura visible); trasladado al `story.md`.
* Regla de conteo definida por decisión delegada QA 09/09/2026 (pendiente de ratificación PO); trasladada al `story.md`.
* Queda abierta como defecto del sistema: ¿qué cuenta `Citas Hoy` que `Próximas Citas` no lista?
