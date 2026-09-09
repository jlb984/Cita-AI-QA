# Priorización de Pruebas: Configuración de disponibilidad semanal

**Historia:** CAQ-11
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-agenda-disponibilidad.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Guardado válido con reemplazo | 1 | 1 | 0.6 | 2.6 | AUTOMATIZAR | Base de los slots. Mitiga R1 (20, Alto) |
| 2 | Rango inválido rechazado | 1 | 1 | 0.9 | 2.9 | AUTOMATIZAR | Validación barata y estable. Mitiga R1 |
| 3 | Solapados, contiguos, medianoche, fallo, día off y mensaje | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R1/R2 |

## Resumen
*   **Candidatos a automatización:** 2
*   **Regresión manual:** 0
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-11 | Guardado y rechazo | R1 del test-plan | `analisis-escenarios.md` |
| CAQ-11 | Candidato 3 | R1/R2 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
