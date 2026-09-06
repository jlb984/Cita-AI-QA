# Reporte de Inspección de Requisitos: Acceso a la URL pública del profesional

**Historia:** CAQ-6
**Fecha:** 04/09/2026
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| CAQ-6-D1 | Contradice al sistema | La tarjeta `Mi enlace de reservas` requerida no fue localizada en la experiencia autenticada observada. | Implementar o verificar la tarjeta permanente, con `Copiar enlace` y `Abrir página`, antes de aprobar. |

## 2. Versión Corregida de la Historia

La versión vigente de `story.md` contiene 9 escenarios verificables e incorpora las decisiones aprobadas de Producto. El requisito quedó definido; no se convierte el comportamiento observado en criterio. La discrepancia de implementación permanece bloqueante hasta contar con nueva evidencia.

## 3. Valoración de Calidad

* **Veredicto:** Bloqueante
* **Riesgo:** Alto

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Criterios y escenarios inspeccionados | `.context\PBI\epics\EPIC-CAQ-2-cuenta-y-activacion-del-profesional\stories\STORY-CAQ-6-acceso-a-url-publica\story.md` |
| Alcance funcional contrastado | `.context/architecture/prd.md` · Feature 1 |
| Reglas vigentes del release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-6 y decisiones transversales aplicables |
| Comportamiento contrastado | **Observado** — producción, 30/08/2026. Evidencia: `.context/architecture/prd.md` · Feature 1, User Journeys y Fuentes. |

## Contradicciones detectadas

* La especificación histórica usa cita.ai/{slug} y la evidencia vigente utiliza https://cita-ai.vercel.app/; Producto adopta este último dominio para el release 1.1. La falta observada de la tarjeta en el dashboard continúa como brecha de implementación.

## Preguntas abiertas

* ¿La ausencia observada de la tarjeta «Mi enlace de reservas» continúa vigente en el build que se entregará a QA?
