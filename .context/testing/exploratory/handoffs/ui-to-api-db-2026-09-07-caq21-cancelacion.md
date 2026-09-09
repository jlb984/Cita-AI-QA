# Handoff UI -> API/DB: CAQ-21 Cancelación (2026-09-07)

```json
{
  "feature": "CAQ-21 cancelación de turno por el profesional",
  "environment": "Producción",
  "ui_actions": [
    {
      "step": "Reserva pública 2026-09-09 10:00 con cliente sintético",
      "expected_api": {"method": "POST", "endpoint": "/api/booking o equivalente público", "expected_status": 201},
      "expected_db": {"table": "appointments", "operation": "INSERT", "where": {"start_time": "2026-09-09T13:00:00+00:00", "status": "confirmed"}}
    },
    {
      "step": "Clic en Cancelar + aceptar confirm",
      "expected_api": {"method": "PATCH", "endpoint": "/api/appointments/{id}", "expected_status": 200},
      "expected_db": {"table": "appointments", "operation": "UPDATE", "where": {"id": "<id turno sintético run 20260907>", "status": "cancelled"}}
    },
    {
      "step": "Navegación nueva a /dashboard",
      "expected_api": {"method": "GET", "endpoint": "/api/appointments", "expected_status": 200},
      "expected_db": {"table": "appointments", "operation": "SELECT", "where": {"id": "<id turno sintético run 20260907>"}}
    },
    {
      "step": "Disponibilidad pública del 09/09",
      "expected_api": {"method": "GET", "endpoint": "/api/public/availability?professionalId=<uuid>&date=2026-09-09", "expected_status": 200},
      "expected_db": {"table": "appointments", "operation": "SELECT", "where": {"start_time": "2026-09-09T13:00:00+00:00"}}
    }
  ],
  "test_data": {
    "appointment_id": "<id del turno sintético run 20260907, ver session notes>",
    "appointment_start": "2026-09-09T13:00:00+00:00",
    "appointment_status_observed": "confirmed (se esperaba cancelled)",
    "client": "sintético descartable run 20260907 (ver notes)",
    "professional_slug": "ver capturas (perfil usado en la sesión)"
  },
  "trace_ids": {
    "request_id": "no expuesto por la UI",
    "correlation_id": "no expuesto por la UI"
  }
}
```

## Qué debe verificar la capa API/DB

1. Qué responde el endpoint de cancelación al confirmar (¿200 con falso éxito? ¿error tragado por la UI?) y si persiste `cancelled` + libera el slot.
2. Origen del contador "Citas Hoy: 3" del dashboard (la API lista 0–1 turnos según el momento).
3. Por qué la página pública ofrece el slot 10:00 del 09/09 aunque la API lo excluye (riesgo de sobrerreserva).
4. Si el intento de cancelación disparó correo (Resend/Supabase) aunque no persistió.
