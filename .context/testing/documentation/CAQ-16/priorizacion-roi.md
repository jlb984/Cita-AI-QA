# Priorización de Pruebas: Consulta pública de horarios

**Historia:** CAQ-16
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-pagina-publica-y-auto-reserva.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Semana con horarios listados | 1 | 1 | 0.7 | 2.7 | AUTOMATIZAR | Vidriera del producto. Mitiga R2 (16, Alto) |
| 2 | Pasados excluidos | 1 | 1 | 0.7 | 2.7 | AUTOMATIZAR | Regresión del hallazgo tras el fix. Mitiga R2 |
| 3 | Ocupados, bloqueados, ventana, corte y fallos | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R1/R2 |

## Resumen
*   **Candidatos a automatización:** 2
*   **Regresión manual:** 0
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-16 | Listado y exclusión de pasados | R2 del test-plan | `analisis-escenarios.md` |
| CAQ-16 | Candidato 3 | R1/R2 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
