# Caso de Prueba: Rutas protegidas tras logout

**ID:** CP-TEMP-03
**Historia:** CAQ-4
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Sesión cerrada tras haber navegado el dashboard (caché cliente con vistas privadas).

## Datos de Prueba
*   Rutas `/dashboard`, `/dashboard/availability`, `/dashboard/clients`.

## Pasos
Manual equivalente: tras logout, navegar directo a cada ruta y con botón Atrás; ninguna debe renderizar contenido privado.

```gherkin
Feature: Protección post-logout

  @Automate @Priority:High
  Scenario: Rutas protegidas tras cerrar sesión
    Given el profesional cerró sesión tras navegar el dashboard
    When navega directo a `/dashboard`, `/dashboard/availability` o `/dashboard/clients`
    Then no se renderiza contenido privado
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-4 | Protección post-logout | CAQ-4-BUG-01 | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 4, score 2.7 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 8–9
