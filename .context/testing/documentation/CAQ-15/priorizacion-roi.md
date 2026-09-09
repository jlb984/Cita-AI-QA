# Priorización de Pruebas: Consulta de próximos turnos

**Historia:** CAQ-15
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-agenda-disponibilidad.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Estado vacío | 1 | 0.5 | 1 | 2.5 | MANUAL | Límite del rango manual (1.5–2.5); chequeo barato |
| 2 | Contadores consistentes | 1 | 1 | 0.8 | 2.8 | AUTOMATIZAR | Regresión del bug de contadores tras el fix |
| 3 | Lista, orden, paginado, búsqueda, filtros y aislamiento | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R4 (15, Alto) y R1 |

## Resumen
*   **Candidatos a automatización:** 1
*   **Regresión manual:** 1
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-15 | Vacío y contadores | — | `analisis-escenarios.md` |
| CAQ-15 | Candidato 3 | R4/R1 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
