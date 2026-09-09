# Análisis de Escenarios: Cancelación por el cliente

**Historia:** CAQ-20 (`Implementación: Sin verificar` — **dicho en el reporte:** los casos se diseñan contra una especificación que nadie confirmó contra el sistema)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-cancelaciones.md` (sin puntos de entrada sin turnos)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Cancelación futura mediante enlace | `story.md` · esc. 1 | Nunca se probó |
| 2 | Funcional | Rechazos: pasado, ya cancelado, 2 horas, token inválido | `story.md` · esc. 2–5 | Nunca se probó |
| 3 | Funcional | Persistir y liberar antes de informar éxito | `story.md` · esc. 6 | Nunca se probó |

## Huecos de Cobertura
* Todos: requieren reserva real + correo con enlace en producción.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-20 | Sin entradas sin turnos | — | `../exploratory/ui/evidence/screenshots/2026-09-09-cancelaciones-dashboard-sin-citas-pass.png` |
| CAQ-20 | Candidatos 1–3 | — | `session-2026-09-09-cancelaciones.md` |

## Sin cobertura
* Ejecución de los candidatos 1–3: requieren reserva sintética + correo sandbox + entorno aislado.
