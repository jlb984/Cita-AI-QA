# Análisis de Escenarios: Acceso a la página pública

**Historia:** CAQ-17 (`Implementación: Sin verificar` — **dicho en el reporte:** los casos se diseñan contra una especificación que nadie confirmó contra el sistema)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-pagina-publica-reserva.md` (perfil válido renderizado)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Página pública válida con identidad y sesiones | Sesión UI | Ejecutado, PASS |
| 2 | Seguridad | Aislamiento de perfiles por slug | `story.md` · esc. 2 | Nunca se probó |
| 3 | Funcional | Slug inexistente con error controlado | `story.md` · esc. 3 | Nunca se probó (seguro de ejecutar: solo lectura) |
| 4 | Seguridad | Datos públicos limitados | `story.md` · esc. 4 | Nunca se probó |
| 5 | Funcional | Perfil sin disponibilidad | `story.md` · esc. 5 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 2–5: el 3 es seguro de ejecutar en la próxima sesión (GET de solo lectura); 2 y 4 requieren segundo profesional o revisión de exposición.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-17 | Perfil válido | — | `../exploratory/ui/evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| CAQ-17 | Candidatos 2–5 | — | `session-2026-09-09-pagina-publica-reserva.md` |

## Sin cobertura
* Ejecución de los candidatos 2–5 según lo indicado.
