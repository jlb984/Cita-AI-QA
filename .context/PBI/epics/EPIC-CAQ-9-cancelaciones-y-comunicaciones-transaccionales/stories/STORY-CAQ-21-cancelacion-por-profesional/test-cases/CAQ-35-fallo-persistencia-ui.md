# Caso de Prueba: Conservación de la UI ante un fallo de persistencia

**ID:** CAQ-35  
**Historia:** CAQ-21  
**Estado de sincronización:** Sincronizado con Jira  
**Ruta:** A  
**Tipo:** Cucumber  
**Prioridad:** Alta  
**Automatizable:** Sí

## Precondiciones

* El profesional de prueba está autenticado y tiene un turno futuro propio visible.
* El entorno de prueba permite simular una respuesta fallida de la operación de cancelación o impedir controladamente su persistencia.

## Datos de Prueba

* Cuenta profesional de prueba configurada según `.env.example`; no registrar credenciales en el caso.
* Turno futuro sintético en estado `confirmed`.
* Respuesta controlada de error o persistencia fallida de la operación de cancelación.

## Escenario automatizable

```gherkin
Feature: Manejo de un fallo persistente al cancelar

  @Automate @Priority:High
  Scenario: No informar éxito cuando la cancelación no se persiste
    Given el profesional autenticado tiene un turno futuro confirmado propio
    And la operación de cancelación no puede persistir cancelled ni liberar el slot
    When confirma la cancelación desde Próximas Citas
    Then la interfaz no informa éxito
    And mantiene el turno visible como confirmado
    And no presenta el horario como liberado
```

## Trazabilidad

| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Ante un fallo de persistencia, no informar éxito ni retirar definitivamente el turno confirmado | CAQ-29 | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 6 |

## Origen

* **Priorización:** `priorizacion-roi.md` · escenario 6, score 2.7.
* **Criterio de aceptación cubierto:** `story.md` · CA-7.
