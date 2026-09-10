# Caso de Prueba: Semana con horarios listados

**ID:** CP-TEMP-01
**Historia:** CAQ-16
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Perfil público con disponibilidad (ruta pública sin sesión).

## Datos de Prueba
*   Fecha con horarios configurados.

## Pasos
Manual equivalente: abrir el perfil, elegir fecha y verificar slots ofrecidos.

```gherkin
Feature: Horarios públicos

  @Automate @Priority:High
  Scenario: Semana con horarios
    Given el perfil público con disponibilidad configurada
    When el cliente elige una fecha con horarios
    Then se listan los slots disponibles del día
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-16 | Slots listados | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.7 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1
