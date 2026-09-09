# Sesión Exploratoria UI: Clientes y límite (CAQ-25/26/27/28)

**Fecha:** 09/09/2026
**Entorno:** Producción (`https://cita-ai.vercel.app/dashboard/clients`)
**Duración:** 10 minutos
**Modo de ejecución:** Playwright MCP
**Misión:** Verificar listado y alta de clientes sin crear PII real ni consumir cupo freemium (10 únicos).

## Escenarios Probados
| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Lista con 2 clientes sintéticos y botón Contactar | PASS | `evidence/screenshots/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |
| 2 | `Nuevo Cliente` no abre diálogo ni produce cambio visible | FAIL | `evidence/screenshots/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |
| 3 | `Contactar` no produce efecto visible en la página | PASS | Sin captura adicional (sin cambio) |
| 4 | Alta manual de cliente | No ejecutado | No hay formulario donde cargarla; y crearla consumiría cupo real |
| 5 | Cliente nuevo número 11 bloqueado | No ejecutado | Requeriría 9 altas reales hasta el tope |
| 6 | Información del Plan Pro al alcanzar el límite | No ejecutado | Sin tope alcanzado no hay dónde observarla |

## Defectos Encontrados
* `Nuevo Cliente` sin respuesta (ver `bug-2026-09-09-nuevo-cliente-sin-respuesta.md`): confirma el hallazgo del PRD y sostiene el Bloqueante de CAQ-26.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-25 | Lista de clientes | — | `evidence/screenshots/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |
| CAQ-26 | Alta manual sin formulario | pendiente | `evidence/screenshots/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |

## Sin cobertura
* Escenarios 4–6 con sus motivos (ver tabla): necesitan altas reales + entorno aislado.

## Transferencia UI -> API/DB
### Acciones Disparadoras
* Nuevo Cliente -> (se esperaba formulario/`POST` clientes; no ocurre nada, sin errores en consola).

### Datos de Prueba Utilizados
* Ninguno creado en esta sesión (cuenta con 2 clientes sintéticos previos).

### Trazas Observadas
* requestId / correlationId no expuestos por la UI.
