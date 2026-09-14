# Caso de Prueba: Email inválido bloqueado en reserva

**ID:** CP-TEMP-02
**Historia:** CAQ-18
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Manual
**Prioridad:** Media
**Automatizable:** No

## Precondiciones
*   Slot seleccionado con formulario visible, sin confirmar.

## Datos de Prueba
*   Email inválido de formato (solo cliente, sin rastro).

## Pasos
| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Informar email sin `@` e intentar confirmar | El envío se bloquea en cliente, sin request ni reserva creada |

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-18 | Email inválido | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 2, score 2.4 (MANUAL)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 4
