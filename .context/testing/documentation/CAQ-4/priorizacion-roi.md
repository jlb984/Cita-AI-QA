# Priorización de Pruebas: Inicio y cierre de sesión

**Historia:** CAQ-4
**Fecha:** 09/09/2026
**Matriz de riesgos usada:** `.context/testing/test-plan-cuenta-y-activacion.md`

## Matriz de Priorización
| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Login válido con redirección | 1 | 1 | 0.9 | 2.9 | AUTOMATIZAR | Sin login no hay producto. Smoke de cada release |
| 2 | Vacíos e inválidas genéricas | 1 | 1 | 0.9 | 2.9 | AUTOMATIZAR | Mitiga R3 (enumeración/abuso) |
| 3 | Logout con redirección | 1 | 1 | 0.9 | 2.9 | AUTOMATIZAR | Mitiga R1 (20, Alto) |
| 4 | Rutas protegidas post-logout | 1 | 1 | 0.7 | 2.7 | AUTOMATIZAR | Regresión del defecto CAQ-4-BUG-01. Mitiga R1 |
| 5 | Reingreso, multi-pestaña, dispositivos y bloqueo | — | — | — | — | PROBAR ANTES DE DECIDIR | Nunca se ejecutó. Mitiga R1 |

## Resumen
*   **Candidatos a automatización:** 4
*   **Regresión manual:** 0
*   **Probar antes de decidir:** 1
*   **Descartados:** 0

## Trazabilidad
| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-4 | Login/logout/validaciones | R1/R3 del test-plan | `analisis-escenarios.md` |
| CAQ-4 | Rutas post-logout | R1 del test-plan | `analisis-escenarios.md` |
| CAQ-4 | Candidato 5 | R1 del test-plan | `analisis-escenarios.md` |

## Sin cobertura
* Ninguna: todo lo no ejecutado está como pendiente explícito.
