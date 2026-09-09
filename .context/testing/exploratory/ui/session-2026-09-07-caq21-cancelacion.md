# Sesión Exploratoria UI: CAQ-21 Cancelación de turno por el profesional

**Fecha:** 2026-09-07
**Entorno:** Producción
**Duración:** ~30 minutos
**Modo de ejecución:** Playwright MCP
**Misión:** Encontrar inconsistencias en el flujo de cancelación de un turno futuro por el profesional (CAQ-21), verificando UI, persistencia y liberación del horario. Sesión autorizada expresamente por el usuario en producción con el usuario del perfil del navegador, incluyendo acciones destructivas y creación de datos sintéticos.

## Escenarios Probados
| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Estado inicial: dashboard sin próximas citas, API vacía | PASS | `evidence/screenshots/2026-09-07-caq21-dashboard-sin-proximas-pass.png` |
| 2 | Crear turno sintético futuro (09/09 10:00) desde el perfil público | PASS | `evidence/screenshots/2026-09-07-caq21-reserva-sintetica-creada-pass.png` |
| 3 | El turno creado aparece en Próximas Citas con acciones Contactar y Cancelar | PASS | `evidence/screenshots/2026-09-07-caq21-turno-futuro-con-accion-cancelar-pass.png` |
| 4 | Diálogo de confirmación con texto exacto y sin campo de motivo | PASS | Diálogo nativo `confirm` observado: "¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer." (Playwright bloquea la captura con el diálogo abierto; estado anterior y posterior evidenciados) |
| 5 | Confirmar cancelación: la UI retira el turno de la lista | PASS (engañoso, ver #6) | `evidence/screenshots/2026-09-07-caq21-turno-retirado-tras-confirmar-pass.png` |
| 6 | Persistencia: tras navegación nueva el turno reaparece y la API lo mantiene `confirmed` | FAIL | `evidence/screenshots/2026-09-07-caq21-turno-reaparece-tras-recarga-fail.png` + `GET /api/appointments` 200 con `status: confirmed` |
| 7 | Liberación del slot: la disponibilidad pública/API excluye 10:00 del 09/09 | FAIL | `evidence/screenshots/2026-09-07-caq21-slot-no-liberado-o-disponible-fail.png` + `GET /api/public/availability` 200 sin el slot 13:00–13:30 UTC |
| 8 | Rechazar cancelación dentro de 2 horas | No ejecutado | Sin turno dentro de la ventana de 2 h; los slots de hoy ya estaban en el pasado al momento de la sesión |
| 9 | Impedir cancelar un turno ajeno | No ejecutado | Sin acceso a un segundo profesional para preparar el caso |
| 10 | Impedir cancelar un turno pasado | No ejecutado | Sin turno pasado asociado a la cuenta |
| 11 | Contador "Citas Hoy: 3" con `GET /api/appointments` devolviendo `[]` | FAIL (hallazgo nuevo, fuera del charter) | Dashboard fotografiado en escenarios 1 y 6; API `[]` antes de crear el turno y con 1 turno después |
| 12 | La página pública renderiza el botón 10:00 del 09/09 aunque la API lo excluye por ocupado | FAIL (hallazgo nuevo; la doble reserva no se ejecutó para no crear más datos) | `evidence/screenshots/2026-09-07-caq21-slot-no-liberado-o-disponible-fail.png` + cuerpos de API citados en notas |

## Defectos Encontrados
* **Bloqueante (reproduce el del 02/09/2026): la cancelación informa éxito en UI pero no persiste.** Tras confirmar, el turno se retira de Próximas Citas, pero una navegación nueva lo muestra otra vez y `GET /api/appointments` lo devuelve `confirmed`. El slot tampoco se libera. Viola los escenarios 1 y 7 de CAQ-21 y la regla de release 1.1 ("la API solo responde éxito después de persistir `cancelled` y liberar el slot").
* **Nuevo: el contador "Citas Hoy: 3" no coincide con la API.** Con `GET /api/appointments` en `[]`, el dashboard muestra 3 programadas para hoy y "Próxima Cita --:--". El origen del contador es desconocido.
* **Nuevo: la página pública ofrece un slot que la API marca ocupado.** Para el 09/09 la UI lista 10:00 (clickeable) mientras `GET /api/public/availability` lo excluye (el turno sintético sigue `confirmed`). Riesgo de sobrerreserva; no se intentó la doble reserva.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Escenario 1: cancelar turno futuro propio | pendiente (bloqueante 02/09 + reproducido 07/09) | `evidence/screenshots/2026-09-07-caq21-turno-reaparece-tras-recarga-fail.png` |
| CAQ-21 | Escenario 4: confirmación con texto exacto, sin motivo | — | Diálogo nativo observado (texto citado arriba) |
| CAQ-21 | Escenario 7: conservar la UI ante fallo persistente | pendiente (la UI informa éxito y retira el turno) | `evidence/screenshots/2026-09-07-caq21-turno-retirado-tras-confirmar-pass.png` |
| CAQ-21 | Escenario 5: ventana de 2 horas | — | No ejecutado |
| CAQ-21 | Escenarios 2 y 3: turno ajeno / pasado | — | No ejecutado |

## Sin cobertura
* Escenarios 5, 2 y 3 de CAQ-21 (ventana de 2 h, turno ajeno, turno pasado): sin datos para prepararlos durante la sesión.
* Motivo opcional de hasta 250 caracteres (escenario 6): la UI no ofrece campo de motivo, coherente con la decisión de release 1.1 (motivo opcional, no exigido); no se verificó persistencia de motivo porque no hay dónde informarlo.
* Doble reserva del slot ocupado: no ejecutada a propósito para no crear más datos en producción.
* Correos de confirmación/cancelación (Resend): fuera del charter UI; la reserva mostró el aviso "Te hemos enviado un email", sin verificar bandeja.
* El error 400 observado en consola corresponde a una sonda manual propia con parámetros incorrectos (`slug`+`date`), no a un defecto de la aplicación; el formato real usa `professionalId`+`date`.

## Transferencia UI -> API/DB
### Acciones Disparadoras
* Reserva pública (fecha + slot + nombre + email → Confirmar Reserva) -> `POST` reserva pública (éxito observado vía redirect a `/booking/success`); crea turno `confirmed` + cliente sintético.
* Clic en Cancelar → diálogo nativo `confirm` -> se esperaba `PATCH/DELETE` sobre el turno con persistencia a `cancelled` + liberación del slot; la red no mostró persistencia y la API mantuvo `confirmed`.
* Navegación nueva a `/dashboard` -> `GET /api/appointments` 200 (el turno reaparece `confirmed`).

### Datos de Prueba Utilizados
* Profesional: slug público del perfil usado en la sesión (visible en las capturas); credenciales fuera del repositorio (perfil del navegador, ver `.env.example`: `TEST_USER_PASSWORD`).
* Cliente sintético: nombre y email descartables con `run_id` 20260907 (valores exactos en las respuestas de API citadas en `evidence/caq21-cancelacion-notes.md`, no reproducidos aquí).
* Turno sintético: 2026-09-09 10:00 America/Buenos_Aires (13:00 UTC), 30 min; id de API citado en notas.
* Nota: los datos sintéticos quedan en producción (sin teardown disponible) y la reserva pudo disparar un correo real al email descartable.

### Trazas Observadas
* `requestId`: no expuesto por la UI.
* `correlationId`: no expuesto por la UI.
* Identificadores disponibles: id del turno en `GET /api/appointments` y `professionalId` en `GET /api/public/availability` (valores en notas, no aquí).
