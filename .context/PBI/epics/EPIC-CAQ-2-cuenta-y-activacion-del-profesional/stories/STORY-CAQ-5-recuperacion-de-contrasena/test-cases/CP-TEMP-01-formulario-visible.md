# Caso de Prueba: Formulario de recupero visible

**ID:** CP-TEMP-01
**Historia:** CAQ-5
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Manual
**Prioridad:** Media
**Automatizable:** No

## Precondiciones
*   Ruta `/forgot-password` accesible, sin enviar nada.

## Datos de Prueba
*   Ninguno (solo verificación visual).

## Pasos
| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Abrir `/forgot-password` | Se muestra campo de correo y botón de envío con texto de enlace seguro |

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-5 | Formulario visible | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.5 (MANUAL)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1 (presentación)
