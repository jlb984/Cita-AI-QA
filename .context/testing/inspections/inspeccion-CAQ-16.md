# Reporte de Inspección de Requisitos: Consulta pública de horarios disponibles

**Historia:** CAQ-16
**Fecha:** 09/09/2026 (reinspección con sesión exploratoria; anterior: 04/09/2026)
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| 1 | Contradice al sistema | RN-03 y 2.3 exigen no ofrecer pasado ni reservar dentro de las 2 horas previas; el 09/09/2026 (~14:36 hora local) se ofrecían slots 09:00–13:30, todos pasados. | Confirmar la zona del profesional y el corte aplicado; si el corte falta, implementarlo. Evidencia: sesión `session-2026-09-09-pagina-publica-reserva.md` |

## 2. Versión Corregida de la Historia

La versión vigente de `story.md` contiene 8 escenarios verificables e incorpora las decisiones aprobadas de Producto, incluidos límites, mensajes, estados y casos borde aplicables. No conserva ambigüedades funcionales abiertas.

## 3. Valoración de Calidad

* **Veredicto:** Aprobado
* **Riesgo:** Alto

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-8-pagina-publica-y-auto-reserva\stories\STORY-CAQ-16-consulta-de-horarios-disponibles\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 3 |
| Reglas vigentes del release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-16 y decisiones transversales aplicables |

## Contradicciones detectadas

* Slots pasados ofrecidos contra RN-03 y ventana de 2.3 (ver defecto 1; registrado también en el `story.md`).

## Preguntas abiertas

* ¿Cuál es la zona del profesional de prueba y qué corte aplica a los slots del día?
