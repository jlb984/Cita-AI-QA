# Bug: [Panel] El botón "Nuevo Cliente" no abre formulario ni produce cambio visible

**ID:** pendiente (local; sin subir a Jira por MCP no disponible para escritura)
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Fecha:** 2026-09-09
**Entorno:** Producción
**Severidad:** Alta
**Prioridad:** Alta

## Descripción
En Producción, con sesión profesional, en `/dashboard/clients` (2 clientes registrados), el botón `Nuevo Cliente` no abre diálogo ni formulario ni produce ningún cambio visible. Sin errores en consola. Reproduce el hallazgo del PRD del 30/08/2026. Bloquea por completo CAQ-26 (no hay otra vía de alta en el panel) y deja sin probar CAQ-27/CAQ-28.

## Pasos para Reproducir
1. Iniciar sesión como profesional (cuenta de prueba; contraseña fuera del repositorio, ver `.env.example`: `TEST_USER_PASSWORD`).
2. Abrir `/dashboard/clients`.
3. Hacer clic en `Nuevo Cliente`.
4. Observar que no aparece ningún formulario.

## Resultados
*   **Esperado:** se abre el alta de cliente (nombre + correo) según `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-26.
*   **Real:** nada ocurre; la lista queda igual.

## Evidencia
*   `.context/testing/exploratory/ui/evidence/screenshots/2026-09-09-clientes-nuevo-sin-dialogo-fail.png`

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-26 | Alta manual de cliente | pendiente | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |

## Referencia Cruzada
*   **Sesión de origen:** `.context/testing/exploratory/ui/session-2026-09-09-clientes-limite.md`
*   **Datos de prueba usados:** ninguno creado; cuenta con 2 clientes sintéticos previos. Sin credenciales ni PII real en este reporte.
*   **IDs de traza:** requestId / correlationId no expuestos por la UI.
