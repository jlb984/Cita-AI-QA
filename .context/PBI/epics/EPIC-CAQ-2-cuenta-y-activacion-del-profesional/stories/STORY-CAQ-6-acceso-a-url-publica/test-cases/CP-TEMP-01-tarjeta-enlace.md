# Caso de Prueba: Tarjeta de enlace público en dashboard

**ID:** CP-TEMP-01
**Historia:** CAQ-6
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Ruta:** A (Jira sin Xray)
**Tipo:** Cucumber
**Prioridad:** Alta
**Automatizable:** Sí

## Precondiciones
*   Sesión profesional activa en `/dashboard`.

## Datos de Prueba
*   Slug esperado de la cuenta de prueba (sin PII real).

## Pasos
Manual equivalente: verificar tarjeta con URL completa, slug, abrir perfil y copiar enlace.

```gherkin
Feature: Enlace público en dashboard

  @Automate @Priority:High
  Scenario: Tarjeta con URL, apertura y copiado
    Given el profesional autenticado está en `/dashboard`
    When mira la tarjeta de enlace público
    Then ve la URL completa con su slug y puede abrirla y copiarla
```

## Trazabilidad
| Historia (US) | Caso / escenario | Bug de origen | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-6 | Tarjeta de enlace | — | `analisis-escenarios.md` |

## Origen
*   **Priorización:** `priorizacion-roi.md` · escenario 1, score 2.8 (AUTOMATIZAR)
*   **Criterio de aceptación cubierto:** `story.md` · esc. 1, 7
