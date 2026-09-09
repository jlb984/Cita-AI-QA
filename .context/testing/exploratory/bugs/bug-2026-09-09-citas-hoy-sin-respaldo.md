# Bug: [Panel] "Citas Hoy: 1" convive con "No tienes citas próximas"

**ID:** pendiente (local; sin subir a Jira por MCP no disponible para escritura)
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Fecha:** 2026-09-09
**Entorno:** Producción
**Severidad:** Media
**Prioridad:** Media

## Descripción
En Producción, con sesión profesional, `/dashboard` muestra `Citas Hoy` con `1 Programada para hoy` y a la vez `Próxima Cita --:--` y `No tienes citas próximas` con `Comparte tu perfil público para empezar a recibir reservas.` Ambas afirmaciones no pueden ser ciertas a la vez, salvo que cuenten cosas distintas sin decirlo.

## Pasos para Reproducir
1. Iniciar sesión como profesional (cuenta de prueba; contraseña fuera del repositorio, ver `.env.example`: `TEST_USER_PASSWORD`).
2. Abrir `/dashboard` y comparar `Citas Hoy` con `Próximas Citas`.

## Resultados
*   **Esperado:** si hay 1 programada para hoy, aparece listada en Próximas Citas con horario y estado — según CAQ-15 y `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-15.
*   **Real:** el contador dice 1 y la lista dice que no hay ninguna.

## Evidencia
*   `.context/testing/exploratory/ui/evidence/screenshots/2026-09-09-agenda-contadores-fail.png`

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-15 | Consultar próximos turnos propios | pendiente | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-09-agenda-contadores-fail.png` |

## Referencia Cruzada
*   **Sesión de origen:** `.context/testing/exploratory/ui/session-2026-09-09-agenda-disponibilidad.md`
*   **Datos de prueba usados:** ninguno creado. Sin credenciales ni PII real en este reporte.
*   **IDs de traza:** requestId / correlationId no expuestos por la UI.
