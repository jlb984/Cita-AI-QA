# Análisis de Escenarios: Consulta pública de horarios

**Historia:** CAQ-16 (`Implementación: Sin verificar` — **dicho en el reporte:** los casos se diseñan contra una especificación que nadie confirmó contra el sistema)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-pagina-publica-reserva.md` (slots y formulario)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Semana con horarios listados | Sesión UI esc. 1 | Ejecutado, PASS (parcial: un día) |
| 2 | Funcional | Exclusión de horarios pasados | Sesión UI (hallazgo: se ofrecían pasados) | Ejecutado, FAIL (sistema) |
| 3 | Funcional | Exclusión de ocupados/bloqueados y semana vacía | `story.md` · esc. 3–5 | Nunca se probó |
| 4 | Funcional | Ventana reservable y corte a 90 días | `story.md` · esc. 6–7 | Nunca se probó |
| 5 | Funcional | Recuperación ante consulta fallida | `story.md` · esc. 8 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 3–5: requieren manipular reservas/bloqueos o forzar fallos en producción.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-16 | Slots del día | — | `../exploratory/ui/evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| CAQ-16 | Pasados ofrecidos | pendiente | `../exploratory/ui/evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| CAQ-16 | Candidatos 3–5 | — | `session-2026-09-09-pagina-publica-reserva.md` |

## Sin cobertura
* Ejecución de los candidatos 3–5 y re-verificación del 2 tras el fix.
