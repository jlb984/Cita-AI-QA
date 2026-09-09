# Caso de Prueba: Restricción de cancelación dentro de dos horas

**ID:** CAQ-34  
**Historia:** CAQ-21  
**Estado de sincronización:** Sincronizado con Jira  
**Ruta:** A  
**Tipo:** Manual  
**Prioridad:** Media  
**Automatizable:** No

## Precondiciones

* El profesional de prueba está autenticado.
* Existe un turno propio cuyo inicio está a menos de dos horas del momento de la prueba.

## Datos de Prueba

* Cuenta profesional de prueba configurada según `.env.example`; no registrar credenciales en el caso.
* Turno sintético dentro de la ventana restringida de dos horas.

## Pasos

| # | Acción | Resultado Esperado |
| :--- | :--- | :--- |
| 1 | Abrir Próximas Citas y seleccionar `Cancelar` en el turno dentro de dos horas | El sistema rechaza la operación. |
| 2 | Revisar el mensaje mostrado | Se muestra `Ya no puedes cancelar este turno desde el panel. Contacta al cliente.` |
| 3 | Revisar el turno y su disponibilidad | El turno conserva su estado y el horario no se libera. |

## Trazabilidad

| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Rechazar la cancelación cuando faltan menos de dos horas | CAQ-21-BUG-02 | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 5 |

## Origen

* **Priorización:** `priorizacion-roi.md` · escenario 5, score 1.8.
* **Criterio de aceptación cubierto:** `story.md` · CA-5.
