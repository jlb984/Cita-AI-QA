# Bug: [Panel] La cancelación del profesional muestra éxito pero no persiste ni libera el slot

**ID:** CAQ-29
**Estado de sincronización:** Sincronizado con Jira
**Fecha:** 2026-09-07
**Entorno:** Producción
**Severidad:** Crítica
**Prioridad:** Alta

## Descripción
En Producción, el profesional cancela un turno futuro propio desde Próximas Citas: la UI pide confirmación ("¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer."), retira el turno de la lista al aceptar, pero el cambio no persiste. Tras una navegación nueva el turno reaparece, `GET /api/appointments` lo mantiene `confirmed` y el slot no se libera. Reproduce el hallazgo del 02/09/2026. Viola los escenarios 1 y 7 de CAQ-21.

## Pasos para Reproducir
1. Ir a `https://cita-ai.vercel.app/` e iniciar sesión como profesional (usuario de prueba del perfil; contraseña fuera del repositorio, ver `.env.example`: `TEST_USER_PASSWORD`).
2. Abrir `/dashboard` y verificar Próximas Citas.
3. Crear un turno futuro desde el perfil público si no hay ninguno (fecha 2026-09-09, slot 10:00 America/Buenos_Aires, cliente sintético descartable).
4. En Próximas Citas, hacer clic en Cancelar del turno y aceptar el diálogo de confirmación.
5. Observar que el turno desaparece de la lista.
6. Navegar de nuevo a `/dashboard` (nueva navegación, no solo re-render).
7. Consultar `GET /api/appointments` y `GET /api/public/availability?professionalId=<uuid>&date=2026-09-09`.

## Resultados
*   **Esperado:** el turno pasa a `cancelled`, no vuelve a mostrarse y el horario vuelve a ofrecerse — según `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3 y `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-21 ("la API solo responde éxito después de persistir `cancelled` y liberar el slot; la UI no puede retirar el turno definitivamente ante una respuesta fallida").
*   **Real:** el turno reaparece en Próximas Citas, la API lo devuelve `status: confirmed` y la disponibilidad excluye el slot 13:00–13:30 UTC. La UI informó un éxito que nunca ocurrió.

## Evidencia
*   `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-futuro-con-accion-cancelar-pass.png`
*   `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-retirado-tras-confirmar-pass.png`
*   `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-reaparece-tras-recarga-fail.png`
*   `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-slot-no-liberado-o-disponible-fail.png`
*   `.context/testing/exploratory/ui/evidence/caq21-cancelacion-notes.md` (cuerpos de API recortados)

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Escenario 1: cancelar turno futuro propio | CAQ-29 | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-reaparece-tras-recarga-fail.png` |
| CAQ-21 | Escenario 7: conservar la UI ante fallo persistente | CAQ-29 | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-retirado-tras-confirmar-pass.png` |

## Referencia Cruzada
*   **Sesión de origen:** `.context/testing/exploratory/ui/session-2026-09-07-caq21-cancelacion.md`
*   **Datos de prueba usados:** turno sintético 2026-09-09 10:00 America/Buenos_Aires con cliente descartable (run 20260907); permanece `confirmed` en producción sin teardown disponible. Sin credenciales ni PII real en este reporte.
*   **IDs de traza:** requestId / correlationId no expuestos por la UI. Identificadores disponibles: id del turno en `GET /api/appointments` y `professionalId` en availability (ver notas de evidencia).
