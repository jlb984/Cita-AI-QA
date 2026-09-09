# Sesión Exploratoria UI: Agenda y disponibilidad (CAQ-11/12/13/15)

**Fecha:** 09/09/2026
**Entorno:** Producción (`https://cita-ai.vercel.app/`)
**Duración:** 40 minutos
**Modo de ejecución:** Playwright MCP
**Misión:** Verificar los flujos de disponibilidad semanal, duración, bloqueos y próximos turnos contra lo escrito, sin dejar estado (configuración restaurada, bloqueo sintético eliminado).

## Escenarios Probados
| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Horario semanal mostrado (lun–vie 09:00–14:00, dom/sáb off) | PASS | `evidence/screenshots/2026-09-09-agenda-horario-semanal-pass.png` |
| 2 | Rango inválido (mar 09:00–08:00) rechazado al guardar | PASS | `evidence/screenshots/2026-09-09-agenda-rango-invalido-pass.png` |
| 3 | Guardado válido + persistencia por recarga | PASS | `evidence/screenshots/2026-09-09-agenda-guardado-pass.png` |
| 4 | Catálogo de duraciones 15–120 con 30 seleccionado | PASS | `evidence/screenshots/2026-09-09-agenda-duracion-catalogo-pass.png` |
| 5 | Alta de bloqueo sintético 24/12/2026 con motivo | PASS | `evidence/screenshots/2026-09-09-agenda-bloqueo-creado-pass.png` |
| 6 | Eliminación de bloqueo con confirmación | PASS | Sin captura del diálogo (texto literal transcripto) |
| 7 | Estado vacío de Próximas Citas con textos exactos | PASS | `evidence/screenshots/2026-09-09-agenda-proximas-vacio-pass.png` |
| 8 | Contadores (`Citas Hoy: 1`) vs lista vacía | FAIL | `evidence/screenshots/2026-09-09-agenda-contadores-fail.png` |
| 9 | Solapamiento, contigüidad, cruce de medianoche | No ejecutado | Mutaría la configuración productiva más allá de lo autorizado |
| 10 | Cambio de duración y remanentes | No ejecutado | Muta la generación de slots en producción |
| 11 | Rango inválido y superpuestos de bloqueo | No ejecutado | Fuera del charter de esta sesión |
| 12 | Lista poblada, búsqueda, filtros, paginado | No ejecutado | Sin datos; crear 20+ turnos en producción está prohibido |

## Defectos Encontrados
* Inconsistencia `Citas Hoy: 1` vs `No tienes citas próximas` con `Próxima Cita --:--` (ver `bug-2026-09-09-citas-hoy-sin-respaldo.md`).

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-11 | Guardar disponibilidad válida | — | `evidence/screenshots/2026-09-09-agenda-guardado-pass.png` |
| CAQ-11 | Rechazar rango inválido | — | `evidence/screenshots/2026-09-09-agenda-rango-invalido-pass.png` |
| CAQ-12 | Catálogo de duraciones | — | `evidence/screenshots/2026-09-09-agenda-duracion-catalogo-pass.png` |
| CAQ-13 | Crear y eliminar bloqueo | — | `evidence/screenshots/2026-09-09-agenda-bloqueo-creado-pass.png` |
| CAQ-15 | Estado vacío | — | `evidence/screenshots/2026-09-09-agenda-proximas-vacio-pass.png` |
| CAQ-15 | Contadores vs lista | pendiente | `evidence/screenshots/2026-09-09-agenda-contadores-fail.png` |

## Sin cobertura
* Escenarios 9–12 con sus motivos (ver tabla).
* Efecto público de la configuración (slots de `/jorge-luis-ecosistemas-403` tras guardar): cubierto parcialmente en la sesión pública del mismo día.

## Transferencia UI -> API/DB
### Acciones Disparadoras
* Guardar Cambios -> `PUT` disponibilidad semanal (reemplazo completo esperado).
* Crear Bloqueo -> `POST` bloqueo con inicio/fin/motivo (esperado).
* Eliminar bloqueo + confirmar -> `DELETE` bloqueo (esperado).

### Datos de Prueba Utilizados
* email: cuenta profesional de prueba (ver `.env.example`: `TEST_USER_EMAIL`; valor fuera del repositorio).
* Bloqueo sintético 24/12/2026 09:00–17:00 motivo `QA sintético`: creado y eliminado en la sesión, sin rastro.

### Trazas Observadas
* requestId / correlationId no expuestos por la UI.
