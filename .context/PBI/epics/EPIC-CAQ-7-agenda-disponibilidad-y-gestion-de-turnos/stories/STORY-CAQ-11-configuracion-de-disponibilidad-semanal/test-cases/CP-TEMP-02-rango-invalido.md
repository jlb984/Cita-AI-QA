# Caso de Prueba: Rango inválido rechazado sin persistir

**ID:** CP-TEMP-02
**Historia:** CAQ-11
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Sesión profesional activa con configuración conocida (para verificar que no cambia).

## Datos de Prueba
*   Bloque con fin anterior al inicio (solo cliente, sin rastro si es rechazado).

## Pasos
Manual equivalente: informar rango inválido, guardar y verificar mensaje + reglas intactas.

```gherkin
Feature: Disponibilidad semanal

  @Automate @Priority:High
  Scenario: Rango inválido rechazado
    Given un bloque con fin anterior a su inicio
    When el profesional intenta guardar
    Then el sistema rechaza y conserva las reglas anteriores
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-11 | Rechazo de rango inválido | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 2, score 2.9 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 2
