# Caso de Prueba: Liberación del horario después de cancelar

**ID:** CAQ-31  
**Historia:** CAQ-21  
**Estado de sincronización:** Sincronizado con Jira  
**Ruta:** A  
**Tipo:** Cucumber  
**Prioridad:** Alta  
**Automatizable:** Sí

## Precondiciones

* El profesional de prueba está autenticado y tiene un turno futuro `confirmed` asociado a su cuenta.
* La disponibilidad pública del profesional puede consultarse para la fecha del turno.

## Datos de Prueba

* Cuenta profesional de prueba configurada según `.env.example`; no registrar credenciales en el caso.
* Turno futuro sintético y su horario público asociado.

## Escenario automatizable

```gherkin
Feature: Liberación del horario cancelado

  @Automate @Priority:High
  Scenario: Volver a ofrecer el horario después de una cancelación persistida
    Given existe un turno futuro confirmado que ocupa un horario público
    When el profesional confirma la cancelación del turno
    Then el estado del turno es cancelled
    And la disponibilidad pública vuelve a ofrecer el horario del turno
```

## Trazabilidad

| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Liberar el horario público después de cancelar un turno propio | CAQ-29 | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 2 |

## Origen

* **Priorización:** `priorizacion-roi.md` · escenario 2, score 2.8.
* **Criterio de aceptación cubierto:** `story.md` · CA-1.
