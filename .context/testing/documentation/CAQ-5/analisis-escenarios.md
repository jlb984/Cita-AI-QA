# Análisis de Escenarios: Recuperación de contraseña

**Historia:** CAQ-5 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-cuenta-y-activacion.md` (formulario visible, sin envío)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Formulario de recupero accesible | Sesión UI | Ejecutado, PASS (solo visual) |
| 2 | Funcional | Solicitud registrada/no registrada/vacía/inválida | `story.md` · esc. 1–4 | Nunca se probó |
| 3 | Funcional | Token vigente/vencido/reutilizado/invalidación | `story.md` · esc. 5–8 | Nunca se probó |
| 4 | Seguridad | Reutilización de contraseña y límites por correo/IP | `story.md` · esc. 9–11 | Nunca se probó |
| 5 | Funcional | Recuperación exitosa completa | `story.md` · esc. 12 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 2–5: dispararían correos reales o manipularían credenciales en producción; no se ejecutaron por restricción de entorno.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-5 | Formulario visible | — | `session-2026-09-09-cuenta-y-activacion.md` (sin captura) |
| CAQ-5 | Candidatos 2–5 | — | `session-2026-09-09-cuenta-y-activacion.md` |

## Sin cobertura
* Ejecución de los candidatos 2–5: requieren correo sandbox y entorno aislado.
