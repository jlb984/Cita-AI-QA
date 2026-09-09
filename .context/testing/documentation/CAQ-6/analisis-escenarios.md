# Análisis de Escenarios: Acceso a la URL pública

**Historia:** CAQ-6 (`Implementación: Implementada` — los casos miden contra algo que existe)
**Fecha:** 09/09/2026
**Sesiones analizadas:**
* `.context/testing/exploratory/ui/session-2026-09-09-cuenta-y-activacion.md` (tarjeta reconfirmada)

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional | Tarjeta con slug, apertura y copiado en dashboard | Sesión UI | Ejecutado, PASS (visual) |
| 2 | Funcional | Composición con dominio vigente y slug | `story.md` · esc. 2–3 | Nunca se probó (requiere alta) |
| 3 | Funcional | Colisión de slug con sufijo | `story.md` · esc. 4 | Nunca se probó (requiere alta) |
| 4 | Funcional | Copia directa y confirmación accesible | `story.md` · esc. 5, 8 | Nunca se probó |
| 5 | Funcional | Enlace durante activación y perfil sin disponibilidad | `story.md` · esc. 6, 9 | Nunca se probó |

## Huecos de Cobertura
* Candidatos 2–5: el clic de copiado/apertura y las altas no se ejecutaron en esta sesión.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-6 | Tarjeta reconfirmada | — | `session-2026-09-09-cuenta-y-activacion.md` |
| CAQ-6 | Candidatos 2–5 | — | `session-2026-09-09-cuenta-y-activacion.md` |

## Sin cobertura
* Clic efectivo en copiar/abrir y casos de alta/colisión: ventana de solo lectura en esta sesión.
