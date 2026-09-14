# Caso de Prueba: Guardar disponibilidad válida

**ID:** CP-TEMP-01
**Historia:** CAQ-11
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Sesión profesional activa en `/dashboard/availability` con configuración conocida.

## Datos de Prueba
*   Intervalos válidos por día (referencia a cuenta de prueba; nunca la credencial en claro).

## Pasos
Manual equivalente: definir bloques válidos, guardar y verificar persistencia por recarga.

```gherkin
Feature: Disponibilidad semanal

  @Automate @Priority:High
  Scenario: Guardado válido con reemplazo
    Given el profesional define bloques con fin posterior al inicio
    When guarda la disponibilidad
    Then el sistema reemplaza las reglas y las persiste
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-11 | Guardar disponibilidad válida | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.6 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1
