# Análisis de Escenarios: Conflicto de reserva concurrente

**Historia:** CAQ-19 (`Implementación: Sin verificar` — **dicho en el reporte:** los casos se diseñan contra una especificación que nadie confirmó contra el sistema)
**Fecha:** 09/09/2026
**Sesiones analizadas:** ninguna cubre esta historia (la concurrencia está prohibida en producción).

## Candidatos para Regresión
| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Concurrencia | Confirmación ganadora y rechazo perdedor | `story.md` · esc. 1–2 | Nunca se probó |
| 2 | Concurrencia | Unicidad bajo carga y entre canales | `story.md` · esc. 3, 5 | Nunca se probó |
| 3 | Funcional | Conflicto informado sin perder datos | `story.md` · esc. 4 | Nunca se probó |

## Huecos de Cobertura
* Todos: las pruebas de concurrencia están prohibidas en producción por regla del proyecto.

## Descartados
* Ninguno.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-19 | Candidatos 1–3 | — | Sin sesiones (prohibido en producción) |

## Sin cobertura
* Todo: requiere entorno aislado con capacidad de carga y datos descartables.
