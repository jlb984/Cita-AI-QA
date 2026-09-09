# Análisis de Escenarios: Bloqueo de períodos puntuales

**Historia:** CAQ-13 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-agenda-disponibilidad.md` (alta y eliminación, esc. 5–6)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Crear bloqueo válido y listarlo | Sesión UI esc. 5 | Ejecutado, PASS |
| 2 | Funcional | Eliminar bloqueo con confirmación | Sesión UI esc. 6 | Ejecutado, PASS |
| 3 | Funcional | Rechazar rango inválido | `story.md` · esc. 2 | Nunca se probó |
| 4 | Funcional | Advertir turnos incluidos antes de guardar | `story.md` · esc. 4 | Nunca se probó |
| 5 | Funcional | Unificar bloqueos superpuestos | `story.md` · esc. 5 | Nunca se probó |
| 6 | Funcional | Rechazar bloqueo con fin en el pasado | `story.md` · esc. 6 (de decisiones) | Nunca se probó |
| 7 | Funcional | Rechazar motivo mayor a 250 caracteres | `story.md` · esc. 7 (decisión delegada pendiente de ratificación) | Nunca se probó |

## Huecos de Cobertura
* Candidatos 3–7: requieren crear bloques (algunos inválidos por diseño) o turnos confirmados en producción.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-13 | Alta de bloqueo | — | `../exploratory/ui/evidence/screenshots/2026-09-09-agenda-bloqueo-creado-pass.png` |
| CAQ-13 | Eliminación de bloqueo | — | Sesión (diálogo transcripto, sin captura) |
| CAQ-13 | Candidatos 3–7 | — | `session-2026-09-09-agenda-disponibilidad.md` |

## Sin cobertura
* Ejecución de los candidatos 3–7: requieren ventana de mutación o entorno aislado.
