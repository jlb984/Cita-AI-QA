# Priorización de Pruebas: Duración estándar de los turnos

**Historia:** CAQ-12
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-agenda-disponibilidad.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Catálogo vigente | 0 | 1 | 1 | 2.0 | MANUAL | La duración casi no cambia; chequeo visual barato. Mitiga R1 |
| 2 | Aplicación, rechazos, remanente y preservación | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R1 |

## Resumen
*   **Candidatos a automatización:** 0
*   **Regresión manual:** 1
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-12 | Catálogo | R1 del test-plan | `analisis-escenarios.md` |
| CAQ-12 | Candidato 2 | R1 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
