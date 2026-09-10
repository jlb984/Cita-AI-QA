# Caso de Prueba: Perfil público válido

**ID:** CP-TEMP-01
**Historia:** CAQ-17
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Slug público vigente (sin sesión).

## Datos de Prueba
*   Nombre y sesiones esperadas del perfil.

## Pasos
Manual equivalente: abrir el slug y verificar identidad, sesiones y modalidad sin datos privados.

```gherkin
Feature: Página pública

  @Automate @Priority:High
  Scenario: Perfil válido
    Given un slug público vigente
    When el cliente lo abre
    Then ve identidad, sesiones y modalidad sin datos privados
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-17 | Perfil válido | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.8 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1
