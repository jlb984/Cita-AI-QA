# Priorización de Pruebas: Acceso a la página pública

**Historia:** CAQ-17
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-pagina-publica-y-auto-reserva.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Perfil válido renderizado | 1 | 1 | 0.8 | 2.8 | AUTOMATIZAR | Puerta de entrada de reservas. Mitiga R3 (15, Alto) |
| 2 | Aislamiento, slug inexistente, límites y perfil sin disponibilidad | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R3 |

## Resumen
*   **Candidatos a automatización:** 1
*   **Regresión manual:** 0
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-17 | Perfil válido | R3 del test-plan | `analisis-escenarios.md` |
| CAQ-17 | Candidato 2 | R3 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna.
