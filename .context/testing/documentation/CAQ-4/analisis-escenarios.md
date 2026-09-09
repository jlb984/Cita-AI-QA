# Análisis de Escenarios: Inicio y cierre de sesión

**Historia:** CAQ-4 (`Implementación: Parcial` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-08-inicio-y-cierre-de-sesion.md` (esc. 1–5)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Login válido con redirección a dashboard | Sesión 08/09 esc. 1 | Ejecutado, PASS |
| 2 | Funcional | Campos vacíos y credenciales inválidas genéricas | Sesión 08/09 esc. 2–3 | Ejecutado, PASS |
| 3 | Funcional | Logout con redirección e invalidación | Sesión 08/09 esc. 4 | Ejecutado, PASS |
| 4 | Seguridad | Rutas protegidas renderizan tras logout | Sesión 08/09 esc. 5 | Ejecutado, FAIL (CAQ-4-BUG-01) |
| 5 | Seguridad | Reingreso post-logout y cierre multi-pestaña | `story.md` · esc. 9–10 | Nunca se probó |
| 6 | Seguridad | Sesiones de otros dispositivos intactas | `story.md` · esc. 11 | Nunca se probó |
| 7 | Seguridad | Bloqueo temporal por intentos | `story.md` · esc. 12 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 5–7: requieren manipular sesiones activas o forzar bloqueo en producción; no se ejecutaron por restricción de entorno.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-4 | Login/logout/validaciones | — | `session-2026-09-08-inicio-y-cierre-de-sesion.md` |
| CAQ-4 | Rutas post-logout | CAQ-4-BUG-01 | `session-2026-09-08-inicio-y-cierre-de-sesion.md` |
| CAQ-4 | Candidatos 5–7 | — | `session-2026-09-08-inicio-y-cierre-de-sesion.md` |

## Sin cobertura
* Ejecución de los candidatos 5–7 y re-verificación del 4 tras el fix: requieren entorno aislado.
