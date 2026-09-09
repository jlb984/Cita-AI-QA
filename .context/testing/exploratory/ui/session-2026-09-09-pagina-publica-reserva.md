# Sesión Exploratoria UI: Página pública y reserva (CAQ-16/17/18/19)

**Fecha:** 09/09/2026
**Entorno:** Producción (`https://cita-ai.vercel.app/jorge-luis-ecosistemas-403`)
**Duración:** 15 minutos
**Modo de ejecución:** Playwright MCP
**Misión:** Recorrer la reserva pública hasta el punto de confirmación sin crearla (una reserva real crea datos y dispara correos).

## Escenarios Probados
| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Perfil público con slots del día (09:00–13:30, falta 10:00) | PASS | `evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| 2 | Selección de slot muestra resumen + formulario (nombre/email) | PASS | `evidence/screenshots/2026-09-09-publica-formulario-resumen-pass.png` |
| 3 | Email inválido bloqueado por validación nativa | PASS | `evidence/screenshots/2026-09-09-publica-formulario-resumen-pass.png` |
| 4 | Confirmar reserva válida | No ejecutado | Crearía turno real + correos en producción |
| 5 | Semana siguiente / navegación de fechas | No ejecutado | Fuera del charter de esta sesión |
| 6 | Concurrencia por el mismo slot | No ejecutado | Prohibido: sin pruebas de concurrencia en producción |
| 7 | Revalidación al confirmar contra slot tomado | No ejecutado | Requiere ejecutar la confirmación (ver 4) |

## Defectos Encontrados
* Todos los slots ofrecidos para hoy (09:00–13:30) estaban en el pasado al momento de la observación (~14:36 hora local): la página ofrece horarios pasados, contra el criterio de no ofrecer pasado.
* El resumen (`miércoles, 9 de septiembre a las 11:00 hs`) no incluye identificador ni abreviatura de zona, contra `.context/product-decisions/decisiones-po-proximo-release.md` · 2.2.
* El slot 10:00 no se ofrece sin explicación visible (podría ser la cita que cuenta `Citas Hoy`; queda como pregunta, no como defecto).

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-16 | Slots del día listados | — | `evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| CAQ-17 | Perfil con sesiones de 30 min | — | `evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| CAQ-18 | Resumen + validación de email | — | `evidence/screenshots/2026-09-09-publica-formulario-resumen-pass.png` |
| CAQ-16 | Slots pasados ofrecidos | pendiente | `evidence/screenshots/2026-09-09-publica-slots-hoy-pass.png` |
| CAQ-18 | Resumen sin zona visible | pendiente | `evidence/screenshots/2026-09-09-publica-formulario-resumen-pass.png` |

## Sin cobertura
* Escenarios 4–7 con sus motivos (ver tabla).

## Transferencia UI -> API/DB
### Acciones Disparadoras
* Seleccionar slot -> `GET` disponibilidad pública por fecha (observado en red RSC; endpoint esperado `/api/public/availability`).
* Confirmar Reserva -> `POST` reserva pública (no ejecutado, esperado).

### Datos de Prueba Utilizados
* Nombre `QA Sintético` + email inválido: solo estado local del formulario, sin rastro.

### Trazas Observadas
* requestId / correlationId no expuestos por la UI.
