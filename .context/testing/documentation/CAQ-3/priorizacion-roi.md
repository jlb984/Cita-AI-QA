# Priorización de Pruebas: Registro del profesional

**Historia:** CAQ-3
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-cuenta-y-activacion.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Email inválido bloqueado | 1 | 0.5 | 0.8 | 2.3 | MANUAL | Validación estable por release. Mitiga R2 |
| 2 | Registro exitoso | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R2 |
| 3 | Vacíos, máximos, duplicado y reglas de clave | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R2/R3 |
| 4 | Slug, normalización y onboarding | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R2/R4 |

## Resumen
*   **Candidatos a automatización:** 0
*   **Regresión manual:** 1
*   **Probar antes de decidir:** 3
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-3 | Email inválido | R2 del test-plan | `analisis-escenarios.md` |
| CAQ-3 | Candidatos 2–4 | R2/R3/R4 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna: todo lo no ejecutado está como pendiente explícito.
