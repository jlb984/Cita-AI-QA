# Sesión Exploratoria UI: Cancelaciones y comunicaciones (CAQ-20/22/23/24)

**Fecha:** 09/09/2026
**Entorno:** Producción (`https://cita-ai.vercel.app/`)
**Duración:** 10 minutos
**Modo de ejecución:** Playwright MCP
**Misión:** Verificar puntos de entrada de cancelación y ausencia del recordatorio sin crear reservas ni disparar correos.

## Escenarios Probados
| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Dashboard sin citas: sin acciones de cancelación visibles | PASS | `evidence/screenshots/2026-09-09-cancelaciones-dashboard-sin-citas-pass.png` |
| 2 | Recordatorio del día anterior: sin UI ni evidencia en el panel | PASS | Coherente con su exclusión del lanzamiento (CAQ-24) |
| 3 | Cancelación por el cliente mediante enlace | No ejecutado | Requiere reserva real + correo con enlace |
| 4 | Cancelación por el profesional (esta cuenta) | No ejecutado | Sin turnos en la cuenta; CAQ-21 tiene sesiones propias del 07/09 |
| 5 | Aviso de cancelación a la contraparte | No ejecutado | Requiere una cancelación real que dispare el aviso |
| 6 | Correo de confirmación de reserva | No ejecutado | Requiere una reserva real que dispare el correo |

## Defectos Encontrados
* Ninguno nuevo en esta sesión. CAQ-21 conserva su bug crítico (CAQ-29) y Bloqueante.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-20 | Sin puntos de entrada sin turnos | — | `evidence/screenshots/2026-09-09-cancelaciones-dashboard-sin-citas-pass.png` |
| CAQ-24 | Ausencia de recordatorio | — | Sin captura (ausencia) |

## Sin cobertura
* Escenarios 3–6 con sus motivos (ver tabla): necesitan reserva real + correo sandbox + entorno aislado.

## Transferencia UI -> API/DB
### Acciones Disparadoras
* (Sin acciones ejecutadas: sin turnos no hay disparadores visibles.)

### Datos de Prueba Utilizados
* Ninguno creado en esta sesión.

### Trazas Observadas
* requestId / correlationId no expuestos por la UI.
