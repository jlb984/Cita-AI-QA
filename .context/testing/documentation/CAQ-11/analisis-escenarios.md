# Análisis de Escenarios: Configuración de disponibilidad semanal

**Historia:** CAQ-11 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-agenda-disponibilidad.md` (8 ejecutados, 1 FAIL ajeno a esta historia, 4 no ejecutados)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Guardar disponibilidad válida con reemplazo completo | Sesión UI esc. 3 | Ejecutado, PASS |
| 2 | Funcional | Rechazar rango fin anterior a inicio sin persistir | Sesión UI esc. 2 | Ejecutado, PASS |
| 3 | Funcional | Rechazar bloques solapados | `story.md` · esc. 3 | Nunca se probó |
| 4 | Funcional | Aceptar bloques contiguos | `story.md` · esc. 4 | Nunca se probó |
| 5 | Funcional | Rechazar bloque que cruza medianoche | `story.md` · esc. 5 | Nunca se probó |
| 6 | Funcional | Conservar configuración ante fallo de guardado | `story.md` · esc. 6 | Nunca se probó |
| 7 | Funcional | Desactivar un día conserva confirmados y oculta slots | `story.md` · esc. 7 (decisión delegada pendiente de ratificación) | Nunca se probó |
| 8 | Funcional | Mensaje de rango inválido igual al texto aprobado | Hallazgo doc-vs-sistema (texto observado difiere) | Nunca se probó (requiere cambio o enmienda) |

## Huecos de Cobertura
* Escenarios 3–7: validaciones y reemplazo parcial sin ejecutar (mutarían la configuración productiva más allá de lo autorizado en la sesión).
* Máximo de intervalos por día: sin tope definido salvo decisión delegada (sin tope funcional).

## Descartados
* Ninguno: todo lo ejecutado alimenta regresión.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-11 | Guardado válido | — | `../exploratory/ui/evidence/screenshots/2026-09-09-agenda-guardado-pass.png` |
| CAQ-11 | Rango inválido | — | `../exploratory/ui/evidence/screenshots/2026-09-09-agenda-rango-invalido-pass.png` |
| CAQ-11 | Candidatos 3–8 | — | `session-2026-09-09-agenda-disponibilidad.md` |

## Sin cobertura
* Ejecución de los candidatos 3–8: requieren ventana con autorización de mutación o entorno aislado.
