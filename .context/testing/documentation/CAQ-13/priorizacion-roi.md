# Priorización de Pruebas: Bloqueo de períodos puntuales

**Historia:** CAQ-13
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-agenda-disponibilidad.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Alta y eliminación de bloqueo | 0.5 | 1 | 0.6 | 2.1 | MANUAL | Uso esporádico; crear/eliminar en cada release ensucia agenda. Mitiga R2 (15, Alto) |
| 2 | Inválidos, superpuestos, aviso, pasado y motivo | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R2 |

## Resumen
*   **Candidatos a automatización:** 0
*   **Regresión manual:** 1
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-13 | Alta y eliminación | R2 del test-plan | `analisis-escenarios.md` |
| CAQ-13 | Candidato 2 | R2 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
