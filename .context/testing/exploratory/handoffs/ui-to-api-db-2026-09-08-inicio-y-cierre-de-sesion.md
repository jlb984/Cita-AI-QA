{
  "feature": "inicio-y-cierre-de-sesion",
  "story_id": "CAQ-4",
  "environment": "Producción",
  "ui_actions": [
    {
      "step": "Envío de formulario en /login con credenciales válidas",
      "expected_api": {
        "method": "POST",
        "endpoint": "/auth/v1/token?grant_type=password",
        "expected_status": 200
      },
      "expected_db": {
        "table": "auth.users",
        "operation": "SELECT",
        "where": {
          "email": "usuario.prueba.1@mailinator.com"
        }
      }
    },
    {
      "step": "Clic en «Salir» en navegación del dashboard",
      "expected_api": {
        "method": "POST",
        "endpoint": "/auth/v1/logout",
        "expected_status": 204
      },
      "expected_db": {
        "table": "auth.sessions",
        "operation": "DELETE"
      }
    }
  ],
  "test_data": {
    "email": "usuario.prueba.1@mailinator.com",
    "role": "professional"
  },
  "trace_ids": {
    "request_id": "req-ui-caq-4-20260908",
    "correlation_id": "corr-caq4-auth-ui"
  }
}
