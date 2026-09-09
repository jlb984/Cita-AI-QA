# Notas de evidencia: CAQ-21 Cancelación (2026-09-07)

Sesión: `../session-2026-09-07-caq21-cancelacion.md`
Entorno: Producción (`https://cita-ai.vercel.app/`). Usuario del perfil del navegador, autorizado por el usuario en sesión.

## Respuestas de API observadas (recortadas, sin PII real)

* `GET /api/appointments` antes de crear el turno: `[]` (200).
* `GET /api/appointments` tras confirmar la cancelación y tras navegación nueva (200):
  `[{"id":"4921f7b1-…","start_time":"2026-09-09T13:00:00+00:00","end_time":"2026-09-09T13:30:00+00:00","status":"confirmed","clients":{"name":"<sintético run 20260907>","email":"<descartable run 20260907>"}}]`
  (id completo y email descartable disponibles en la sesión de Playwright; no se reproducen íntegros aquí).
* `GET /api/public/availability?professionalId=<uuid>&date=2026-09-09` (200): slots 09:00, 09:30, 10:30, 11:00, 11:30, 12:00, 12:30, 13:00, 13:30. Falta 10:00 (13:00–13:30 UTC), coherente con el turno aún `confirmed`.
* La página pública del 09/09, habiendo hecho `GET .../availability?...&date=2026-09-09` 200, siguió renderizando el botón 10:00 clickeable.

## Secuencia UI (rutas y títulos)

1. `/` → 308 → `/login` ("Iniciar Sesión - CITA AI") → login → `/dashboard` ("CITA AI - Gestión de Citas").
2. Perfil público `/jorge-luis-ecosistemas-403` ("Perfil Público"): fecha 2026-09-09, slot 10:00, formulario de reserva → `/booking/success?date=2026-09-09&time=10%3A00&professional=…` ("¡Cita Confirmada!").
3. `/dashboard`: Próximas Citas con el turno sintético (Contactar / Cancelar) → Cancelar → `confirm` nativo con el texto de CAQ-21 escenario 4 → aceptar → lista vacía ("No tienes citas próximas").
4. Navegación nueva a `/dashboard`: el turno reaparece (Contactar / Cancelar).

## Consola y red

* Consola durante el flujo de cancelación: 0 errores propios de la app. El único 400 registrado fue una sonda manual con parámetros incorrectos (`slug` en lugar de `professionalId`), no un defecto.
* Red sin 500/404 en el flujo (200 en token, dashboard, availability; 308 raíz → login).

## Limpieza pendiente

* El turno sintético del 09/09 10:00 y su cliente descartable permanecen `confirmed` en producción: la cancelación no persiste y no hay teardown documentado.
* No se intentó borrarlos desde la UI (la única vía, Cancelar, es la que falla) ni por API/DB en esta capa.
