{
  "feature": "cancelacion-por-profesional",
  "story_id": "CAQ-21",
  "environment": "Producción",
  "ui_actions": [
    {
      "step": "Clic en «Cancelar» en tarjeta de cita futura en /dashboard y aceptar diálogo de confirmación",
      "expected_api": {
        "method": "PATCH",
        "endpoint": "/api/appointments",
        "expected_status": 200,
        "payload": {
          "status": "cancelled"
        }
      },
      "expected_db": {
        "table": "appointments",
        "operation": "UPDATE",
        "where": {
          "id": "appointment_id",
          "professional_id": "current_user_id"
        },
        "set": {
          "status": "cancelled"
        }
      }
    },
    {
      "step": "Recarga del dashboard tras cancelación para verificar persistencia",
      "expected_api": {
        "method": "GET",
        "endpoint": "/api/appointments",
        "expected_status": 200
      },
      "expected_db": {
        "table": "appointments",
        "operation": "SELECT",
        "where": {
          "professional_id": "current_user_id",
          "status": "confirmed"
        }
      }
    }
  ],
  "test_data": {
    "email": "usuario.prueba.1@mailinator.com",
    "role": "professional",
    "appointment_type": "Prueba QA 1"
  },
  "trace_ids": {
    "request_id": "req-ui-caq-21-20260907",
    "correlation_id": "corr-caq21-ui-flow"
  }
}
