# Caso de Prueba: Validaciones de login y logout

**ID:** CP-TEMP-02
**Historia:** CAQ-4
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Sesión cerrada para validaciones; sesión activa para logout.

## Datos de Prueba
*   Email sintético inexistente y contraseña errónea (solo intentos, sin bloqueo).

## Pasos
Manual equivalente: campos vacíos e inválidos bloqueados con mensaje genérico; logout redirige a `/login` con navegación pública.

```gherkin
Feature: Validaciones de login y logout

  @Automate @Priority:High
  Scenario: Campos vacíos e inválidos bloqueados
    Given el profesional está en `/login` sin sesión
    When intenta enviar vacío o con credenciales inválidas
    Then no inicia sesión y ve mensaje genérico sin revelar el campo

  @Automate @Priority:High
  Scenario: Cierre de sesión con redirección
    Given el profesional autenticado está en `/dashboard`
    When cierra sesión
    Then es redirigido a `/login` con navegación pública
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-4 | Vacíos, inválidas y logout | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenarios 2–3, score 2.9 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 2–5, 7
