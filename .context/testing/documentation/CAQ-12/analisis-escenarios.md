# Análisis de Escenarios: Duración estándar de los turnos

**Historia:** CAQ-12 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-agenda-disponibilidad.md` (catálogo observado, esc. 4)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Catálogo 15–120 ofrecido con valor vigente | Sesión UI esc. 4 | Ejecutado, PASS |
| 2 | Funcional | Aplicar duración válida y dividir bloques | `story.md` · esc. 1 | Nunca se probó |
| 3 | Funcional | Rechazar duración no positiva | `story.md` · esc. 2 | Nunca se probó |
| 4 | Funcional | No ofrecer remanente incompleto | `story.md` · esc. 3 | Nunca se probó |
| 5 | Funcional | Rechazar duración fuera de catálogo | `story.md` · esc. 4 | Nunca se probó |
| 6 | Funcional | Preservar turnos existentes al cambiar duración | `story.md` · esc. 5 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 2–6: cambiar la duración muta la generación de slots en producción; no se ejecutaron por restricción de entorno.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-12 | Catálogo vigente | — | `../exploratory/ui/evidence/screenshots/2026-09-09-agenda-duracion-catalogo-pass.png` |
| CAQ-12 | Candidatos 2–6 | — | `session-2026-09-09-agenda-disponibilidad.md` |

## Sin cobertura
* Ejecución de los candidatos 2–6: requieren entorno aislado o ventana de mutación autorizada.
