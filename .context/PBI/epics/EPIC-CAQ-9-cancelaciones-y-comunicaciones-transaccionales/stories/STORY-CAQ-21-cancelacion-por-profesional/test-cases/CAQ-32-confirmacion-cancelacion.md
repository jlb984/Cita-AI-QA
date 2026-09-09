# Caso de Prueba: Confirmación de cancelación sin motivo obligatorio

**ID:** CAQ-32  
**Historia:** CAQ-21  
**Estado de sincronización:** Sincronizado con Jira  
**Ruta:** A  
**Tipo:** Manual  
**Prioridad:** Media  
**Automatizable:** No

## Precondiciones

* El profesional de prueba está autenticado.
* Existe un turno futuro propio visible en Próximas Citas.

## Datos de Prueba

* Cuenta profesional de prueba configurada según `.env.example`; no registrar credenciales en el caso.
* Turno futuro sintético propio.

## Pasos

| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Abrir Próximas Citas y seleccionar `Cancelar` en un turno futuro propio | Se muestra un diálogo de confirmación. |
| 2 | Revisar el texto del diálogo | El texto es `¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.` |
| 3 | Revisar los controles disponibles | No se exige completar un motivo para continuar. |

## Trazabilidad

| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Mostrar el texto exacto de confirmación y no exigir un motivo | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 3 |

## Origen

* **Priorización:** `priorizacion-roi.md` · escenario 3, score 2.0.
* **Criterio de aceptación cubierto:** `story.md` · CA-4.
