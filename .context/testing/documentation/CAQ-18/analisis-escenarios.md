# Análisis de Escenarios: Confirmación de reserva sin cuenta

**Historia:** CAQ-18 (`Implementación: Sin verificar` — **dicho en el reporte:** los casos se diseñan contra una especificación que nadie confirmó contra el sistema)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-pagina-publica-reserva.md` (resumen y validación, sin confirmar)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Resumen con fecha/hora y formulario visible | Sesión UI | Ejecutado, PASS |
| 2 | Funcional | Correo inválido bloqueado sin request | Sesión UI | Ejecutado, PASS |
| 3 | Funcional | Confirmación válida con estado `confirmed` | `story.md` · esc. 1 | Nunca se probó |
| 4 | Funcional | Rechazos de nombre/correo vacíos | `story.md` · esc. 2–3 | Nunca se probó |
| 5 | Funcional | Revalidación de slot tomado y detalle | `story.md` · esc. 5–6 | Nunca se probó |
| 6 | Funcional | Idempotencia ante reintento | `story.md` · esc. 7 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 3–6: confirmar crearía turnos y correos reales en producción; no se ejecutaron por restricción de entorno.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-18 | Resumen y validación | — | `../exploratory/ui/evidence/screenshots/2026-09-09-publica-formulario-resumen-pass.png` |
| CAQ-18 | Candidatos 3–6 | — | `session-2026-09-09-pagina-publica-reserva.md` |

## Sin cobertura
* Ejecución de los candidatos 3–6: requieren entorno aislado o reserva sintética autorizada con teardown.
