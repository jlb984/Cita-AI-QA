# Priorización de Pruebas: Acceso a la URL pública

**Historia:** CAQ-6
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-cuenta-y-activacion.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Tarjeta con slug, apertura y copiado | 1 | 1 | 0.8 | 2.8 | AUTOMATIZAR | Sin URL localizable no hay activación. Mitiga R4 (16, Alto) |
| 2 | Composición, colisión, activación y perfil sin disponibilidad | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R4 |

## Resumen
*   **Candidatos a automatización:** 1
*   **Regresión manual:** 0
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-6 | Tarjeta reconfirmada | R4 del test-plan | `analisis-escenarios.md` |
| CAQ-6 | Candidato 2 | R4 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
