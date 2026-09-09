# Caso de Prueba: Registro rechaza correo con formato inválido

**ID:** CP-TEMP-01
**Historia:** CAQ-3
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Manual
**Prioridad:** Media
**Automatizable:** No

## Precondiciones
*   Sesión cerrada, ruta `/login` accesible, sin cuenta creada.

## Datos de Prueba
*   Email inválido de formato (solo cliente, sin rastro en servidor).

## Pasos
| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Abrir `/login` y pasar a registro | Se muestra el formulario de registro |
| 2 | Informar email sin `@` e intentar enviar | El envío se bloquea en cliente, sin request ni cuenta creada |

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-3 | Rechazo de correo inválido | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.3 (MANUAL)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 6
