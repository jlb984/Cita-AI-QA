# Priorización de Pruebas: Recuperación de contraseña

**Historia:** CAQ-5
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-cuenta-y-activacion.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Formulario visible | 1 | 0.5 | 1 | 2.5 | MANUAL | Presencia por release; sin lógica que scriptear |
| 2 | Solicitudes, tokens, reutilización y límites | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R3 (15, Alto) |

## Resumen
*   **Candidatos a automatización:** 0
*   **Regresión manual:** 1
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-5 | Formulario visible | — | `analisis-escenarios.md` |
| CAQ-5 | Candidato 2 | R3 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
