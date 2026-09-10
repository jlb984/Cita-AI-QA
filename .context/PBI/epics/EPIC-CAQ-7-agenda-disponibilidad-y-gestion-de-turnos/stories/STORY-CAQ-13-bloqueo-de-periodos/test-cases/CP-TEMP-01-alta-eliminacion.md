# Caso de Prueba: Alta y eliminación de bloqueo

**ID:** CP-TEMP-01
**Historia:** CAQ-13
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Manual
**Prioridad:** Alta
**Automatizable:** No

## Precondiciones
*   Sesión profesional activa, sin bloqueos (verificar `No hay bloqueos activos`).

## Datos de Prueba
*   Bloqueo sintético futuro con motivo (crear y eliminar en la misma ejecución, sin rastro).

## Pasos
| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Crear bloqueo futuro con motivo | Aparece listado con fecha, horas y motivo |
| 2 | Eliminarlo aceptando la confirmación | Vuelve `No hay bloqueos activos` |

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-13 | Alta y eliminación | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.1 (MANUAL)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1 y 3
