# Reporte de Smoke Test

**Fecha:** 2026-09-07
**Entorno:** Producción
**URL:** https://cita-ai.vercel.app/
**Modo de ejecución:** Playwright MCP
**Estado:** PASSED

## Verificaciones
| # | Verificación | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | Carga de la portada | PASS | `.context/testing/exploratory/smoke/evidence/smoke-portada-2026-09-07-produccion.png` |
| 2 | Título de la página | PASS | Título observado: "Iniciar Sesión - CITA AI" en `/login`; tras login: "CITA AI - Gestión de Citas" en `/dashboard` |
| 3 | Inicio de sesión | PASS | `.context/testing/exploratory/smoke/evidence/smoke-dashboard-2026-09-07-produccion.png` — redirige a `/dashboard` y renderiza Dashboard |

Notas de ejecución:
* Navegación a la URL raíz redirige a `/login` (308 esperado raíz → login, luego 200 en `/login`). Sin errores 500/404.
* Consola del navegador: 0 errores, 0 avisos.
* Red observada: `token?grant_type=password` 200, `dashboard` 200, `speed-insights/vitals` 200. Sin 500/404.
* Login ejecutado con el usuario ya cargado en el perfil del navegador, por indicación del usuario en sesión. No se escriben credenciales en este reporte; la contraseña vive fuera del repositorio (ver `.env.example`: `TEST_USER_PASSWORD` / perfil del navegador).
* Alcance estrictamente de solo lectura: no se crearon ni modificaron citas, clientes, disponibilidad ni reservas, dado que el entorno es producción con usuarios reales.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| N/A — sanidad general | Carga de la portada | — | `.context/testing/exploratory/smoke/evidence/smoke-portada-2026-09-07-produccion.png` |
| N/A — sanidad general | Título de la página | — | Snapshot Playwright `/login` y `/dashboard` (ver notas) |
| N/A — sanidad general | Inicio de sesión con usuario del perfil | — | `.context/testing/exploratory/smoke/evidence/smoke-dashboard-2026-09-07-produccion.png` |

## Sin cobertura
* No se verificaron flujos de reserva pública, disponibilidad, clientes ni cancelaciones: fuera del alcance del smoke.
* No se verificó compatibilidad multi-navegador / multi-dispositivo: sin matriz acordada (ver `environments.md` §1).
* No se ejecutaron escrituras ni pruebas de API/DB: entorno de producción sin aislamiento; pendientes las misiones exploratorias correspondientes.
