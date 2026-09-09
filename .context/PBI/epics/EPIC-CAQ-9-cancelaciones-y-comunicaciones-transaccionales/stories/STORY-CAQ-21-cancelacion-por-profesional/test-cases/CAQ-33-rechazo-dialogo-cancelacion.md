# Caso de Prueba: Rechazo de la cancelación en el diálogo

**ID:** CAQ-33  
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
* Turno futuro sintético propio en estado `confirmed`.

## Pasos

| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Seleccionar `Cancelar` en el turno futuro | Se muestra el diálogo de confirmación. |
| 2 | Seleccionar `Cancelar` o rechazar la confirmación | El diálogo se cierra sin iniciar la cancelación. |
| 3 | Revisar Próximas Citas y el estado del turno | El turno permanece visible, sin cambios de estado ni de disponibilidad. |

## Trazabilidad

| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Rechazar la cancelación en el diálogo y conservar el turno | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 4 |

## Origen

* **Priorización:** `priorizacion-roi.md` · escenario 4, score 1.9.
* **Criterio de aceptación cubierto:** `story.md` · CA-4.
