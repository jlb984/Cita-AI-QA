# Priorización de Pruebas: Confirmación de reserva sin cuenta

**Historia:** CAQ-18
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-pagina-publica-y-auto-reserva.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Resumen visible | 1 | 0.5 | 0.9 | 2.4 | MANUAL | Presentación estable por release |
| 2 | Email inválido bloqueado | 1 | 0.5 | 0.9 | 2.4 | MANUAL | Validación barata por release |
| 3 | Confirmación, vacíos, revalidación e idempotencia | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R1 (20, Alto) y R4 |

## Resumen
*   **Candidatos a automatización:** 0
*   **Regresión manual:** 2
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-18 | Resumen y validación | — | `analisis-escenarios.md` |
| CAQ-18 | Candidato 3 | R1/R4 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
