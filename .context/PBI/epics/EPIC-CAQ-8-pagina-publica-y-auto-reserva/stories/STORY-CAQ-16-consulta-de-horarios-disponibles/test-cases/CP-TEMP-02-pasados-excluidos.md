# Caso de Prueba: Pasados excluidos de la oferta

**ID:** CP-TEMP-02
**Historia:** CAQ-16
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Perfil público, fecha de hoy con franja ya pasada.

## Datos de Prueba
*   Hora de observación para comparar contra slots ofrecidos.

## Pasos
Manual equivalente: verificar que ningún slot con inicio pasado (ni dentro de 2 h) se ofrece.

```gherkin
Feature: Horarios públicos

  @Automate @Priority:High
  Scenario: Pasados excluidos
    Given la fecha de hoy con franja pasada
    When el cliente consulta los slots
    Then no se ofrece ningún horario ya pasado
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-16 | Pasados ofrecidos (hallazgo 09/09) | pendiente | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 2, score 2.7 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 2
