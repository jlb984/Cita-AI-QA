# Caso de Prueba: Cancelación persistente de un turno futuro propio

**ID:** CAQ-30  
**Historia:** CAQ-21  
**Estado de sincronización:** Sincronizado con Jira  
**Ruta:** A  
**Tipo:** Cucumber  
**Prioridad:** Alta  
**Automatizable:** Sí

## Precondiciones

* El profesional de prueba está autenticado y tiene un turno futuro `confirmed` asociado a su cuenta.
* El entorno permite consultar el estado del turno mediante la API y realizar una navegación nueva del dashboard.

## Datos de Prueba

* Cuenta profesional de prueba configurada según `.env.example`; no registrar credenciales en el caso.
* Turno futuro sintético asociado al profesional.

## Escenario automatizable

```gherkin
Feature: Cancelación persistente de un turno futuro propio

  @Automate @Priority:High
  Scenario: Persistir la cancelación después de una navegación nueva
    Given el profesional autenticado tiene un turno futuro confirmado propio
    When inicia y confirma la cancelación desde Próximas Citas
    Then el estado persistido del turno es cancelled
    And el turno no aparece en Próximas Citas
    When navega nuevamente al dashboard
    Then el turno no vuelve a aparecer como confirmado
```

## Trazabilidad

| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Cancelar un turno futuro propio, persistir `cancelled` y retirarlo después de una navegación nueva | CAQ-29 | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 1 |

## Origen

* **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.8.
* **Criterio de aceptación cubierto:** `story.md` · CA-1.
