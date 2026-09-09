# Análisis de Escenarios: Consulta de próximos turnos

**Historia:** CAQ-15 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-agenda-disponibilidad.md` (estado vacío esc. 7, contadores esc. 8 FAIL)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Estado vacío con textos exactos | Sesión UI esc. 7 | Ejecutado, PASS |
| 2 | Funcional | Contadores consistentes con la lista | Sesión UI esc. 8 | Ejecutado, FAIL (`bug-2026-09-09-citas-hoy-sin-respaldo.md`) |
| 3 | Funcional | Consultar próximos turnos propios con horario y estado | `story.md` · esc. 1 | Nunca se probó |
| 4 | Funcional | Mostrar y ordenar turnos con datos completos | `story.md` · esc. 4 | Nunca se probó |
| 5 | Funcional | Paginar de a 20 | `story.md` · esc. 5 | Nunca se probó |
| 6 | Funcional | Buscar por nombre | `story.md` · esc. 6 | Nunca se probó |
| 7 | Funcional | Filtrar por rango de fechas | `story.md` · esc. 7 | Nunca se probó |
| 8 | Funcional | Excluir cancelados | `story.md` · esc. 8 | Nunca se probó |
| 9 | Seguridad | No exponer turnos de otro profesional | `story.md` · esc. 3 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 3–9: sin datos en la cuenta; crear 20+ turnos en producción está prohibido.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-15 | Estado vacío | — | `../exploratory/ui/evidence/screenshots/2026-09-09-agenda-proximas-vacio-pass.png` |
| CAQ-15 | Contadores vs lista | pendiente | `../exploratory/ui/evidence/screenshots/2026-09-09-agenda-contadores-fail.png` |
| CAQ-15 | Candidatos 3–9 | — | `session-2026-09-09-agenda-disponibilidad.md` |

## Sin cobertura
* Ejecución de los candidatos 3–9 y re-verificación del 2 tras el fix: requieren datos y entorno aislado.
