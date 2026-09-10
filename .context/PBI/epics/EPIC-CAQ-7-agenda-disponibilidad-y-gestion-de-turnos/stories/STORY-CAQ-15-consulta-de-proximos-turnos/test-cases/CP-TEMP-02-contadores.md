# Caso de Prueba: Contadores consistentes con la lista

**ID:** CP-TEMP-02
**Historia:** CAQ-15
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Sesión profesional activa en `/dashboard`.

## Datos de Prueba
*   Conteo esperado según turnos `confirmed` del día en zona del profesional.

## Pasos
Manual equivalente: comparar `Citas Hoy` contra `Próximas Citas`; todo lo contado debe listarse.

```gherkin
Feature: Próximos turnos

  @Automate @Priority:High
  Scenario: Contadores consistentes
    Given el dashboard con citas de hoy
    When se comparan los contadores con la lista
    Then todo turno contado aparece listado con horario y estado
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-15 | Contadores vs lista | pendiente (bug local 09/09) | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 2, score 2.8 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1
