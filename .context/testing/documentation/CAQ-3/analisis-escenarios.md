# Análisis de Escenarios: Registro del profesional

**Historia:** CAQ-3 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-cuenta-y-activacion.md` (validación nativa de email)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Rechazo de correo con formato inválido sin request | Sesión UI | Ejecutado, PASS |
| 2 | Funcional | Registro exitoso con bienvenida y URL única | `story.md` · esc. 1 | Nunca se probó |
| 3 | Funcional | Rechazos de campos vacíos y máximos | `story.md` · esc. 2–5, 7 | Nunca se probó |
| 4 | Funcional | Rechazo de correo duplicado y reglas de contraseña | `story.md` · esc. 8–11 | Nunca se probó |
| 5 | Funcional | Colisión de slug, normalización y onboarding | `story.md` · esc. 12–15 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 2–5: crearían cuentas reales en producción; no se ejecutaron por restricción de entorno.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-3 | Email inválido bloqueado | — | `../exploratory/ui/evidence/screenshots/2026-09-09-cuenta-registro-email-invalido-pass.png` |
| CAQ-3 | Candidatos 2–5 | — | `session-2026-09-09-cuenta-y-activacion.md` |

## Sin cobertura
* Ejecución de los candidatos 2–5: requieren entorno aislado o cuenta descartable autorizada.
