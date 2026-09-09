# Priorización de Pruebas: Conflicto de reserva concurrente

**Historia:** CAQ-19
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-pagina-publica-y-auto-reserva.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Ganadora, perdedora, unicidad e información | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó (prohibido en producción). Mitiga R1 (20, Alto) |

## Resumen
*   **Candidatos a automatización:** 0
*   **Regresión manual:** 0
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-19 | Candidato 1 | R1 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Todo lo ejecutable futuro requiere entorno aislado con carga.
