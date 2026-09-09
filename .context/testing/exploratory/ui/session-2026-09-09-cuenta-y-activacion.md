# Sesión Exploratoria UI: Cuenta y activación (CAQ-3/5/6)

**Fecha:** 09/09/2026
**Entorno:** Producción (`https://cita-ai.vercel.app/`)
**Duración:** 15 minutos
**Modo de ejecución:** Playwright MCP
**Misión:** Observar registro, recuperación y URL pública sin crear cuentas ni disparar correos (producción, sin entorno aislado).

## Escenarios Probados
| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | `/login` renderiza con sesión activa (sin redirigir al dashboard) | PASS | Observado en snapshot; sin captura dedicada |
| 2 | Toggle a registro: formulario nombre + email + contraseña | PASS | `evidence/screenshots/2026-09-09-cuenta-registro-email-invalido-pass.png` |
| 3 | Email inválido bloqueado por validación nativa (sin request) | PASS | `evidence/screenshots/2026-09-09-cuenta-registro-email-invalido-pass.png` |
| 4 | Submit con campos vacíos: sin efecto | PASS | Sin captura (sin cambio visible) |
| 5 | Formulario de recupero (`/forgot-password`) visible con sesión | PASS | Sin captura (timeout de screenshot; reintentable) |
| 6 | URL pública visible en dashboard con slug y copiado | PASS | `evidence/screenshots/2026-09-09-agenda-contadores-fail.png` (pantalla completa del dashboard) |
| 7 | Crear cuenta válida | No ejecutado | Crearía una cuenta real en producción |
| 8 | Enviar enlace de recupero | No ejecutado | Dispararía un correo real |
| 9 | Duplicados, límites de contraseña, enumeración de emails | No ejecutado | Requieren envíos reales o APIs; fuera de UI-solo en producción |

## Defectos Encontrados
* Ninguno nuevo. El hallazgo previo de rutas de dashboard renderizadas con/sin sesión (CAQ-4) sigue vigente y tiene sesión propia del 08/09.

## Trazabilidad
| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-3 | Validación de email inválido | — | `evidence/screenshots/2026-09-09-cuenta-registro-email-invalido-pass.png` |
| CAQ-5 | Formulario de recupero visible | — | Sin captura |
| CAQ-6 | URL pública en dashboard | — | `evidence/screenshots/2026-09-09-agenda-contadores-fail.png` |

## Sin cobertura
* Escenarios 7–9 con sus motivos (ver tabla).
* Las capturas usan cuenta profesional de prueba; sin PII real registrada.

## Transferencia UI -> API/DB
### Acciones Disparadoras
* Registrarse -> `POST` registro (no ejecutado, esperado).
* Enviar enlace -> `POST` recupero (no ejecutado, esperado).

### Datos de Prueba Utilizados
* email inválido `sin-arroba`: bloqueado en cliente, sin rastro en servidor.

### Trazas Observadas
* requestId / correlationId no expuestos por la UI.
