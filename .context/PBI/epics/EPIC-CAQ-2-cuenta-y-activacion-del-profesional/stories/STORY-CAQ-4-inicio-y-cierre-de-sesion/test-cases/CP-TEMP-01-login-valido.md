# Caso de Prueba: Login válido con redirección

**ID:** CP-TEMP-01
**Historia:** CAQ-4
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Cuenta profesional de prueba (ver `.env.example`: `TEST_USER_EMAIL`; valor fuera del repositorio).

## Datos de Prueba
*   Referencia a cuenta de prueba; nunca la credencial en claro.

## Pasos
Manual equivalente: informar credenciales válidas y verificar redirección a `/dashboard` con menú autenticado.

```gherkin
Feature: Inicio de sesión seguro

  @Automate @Priority:High
  Scenario: Acceso con credenciales válidas
    Given el profesional está en `/login` sin sesión
    When ingresa sus credenciales válidas
    Then es redirigido a `/dashboard` con el menú autenticado
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-4 | Inicio de sesión exitoso | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.9 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1
