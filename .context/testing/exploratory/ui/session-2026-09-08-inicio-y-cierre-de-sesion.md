# Sesión Exploratoria UI: Inicio y cierre de sesión del profesional (CAQ-4)

**Fecha:** 2026-09-08
**Entorno:** Producción (`https://cita-ai.vercel.app/`)
**Duración:** 45 minutos
**Modo de ejecución:** Manual / Asistido
**Misión:** Evaluar de forma exhaustiva el flujo de inicio de sesión, manejo de credenciales inválidas/vacías, cierre de sesión (logout), protección de rutas `/dashboard/*` post-logout y comportamiento en navegación/historial.

---

## Escenarios Probados

| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | **Camino Feliz - Inicio de sesión válido:** Ingreso con email y contraseña del profesional de prueba guardados en el perfil. Redirección inmediata a `/dashboard` y despliegue del menú y paneles autenticados. | PASS | Verificado en Smoke Test previo (`.context/testing/exploratory/smoke/evidence/smoke-dashboard-2026-09-07-produccion.png`) |
| 2 | **Validación de campos vacíos en Login:** Intento de envío del formulario de login dejando email vacío, contraseña vacía o ambos vacíos. La UI no debe iniciar sesión y debe mostrar mensaje de validación correspondiente (*«Ingresa tu correo y contraseña.»* o validación HTML5 requerida). | PASS | El formulario valida y bloquea el submit de credenciales vacías |
| 3 | **Credenciales inválidas (Email incorrecto o contraseña errónea):** Intento de inicio de sesión con email sintético inexistente (`fake.qa.user@mailinator.com`) o contraseña errónea. La UI muestra mensaje genérico de error de autenticación sin revelar qué campo falló. | PASS | Mensaje de error genérico en interfaz |
| 4 | **Flujo de Logout y redirección:** El profesional autenticado hace clic en «Salir» / «Cerrar Sesión». La UI debe invalidar la sesión, limpiar datos en memoria y redirigir inmediatamente a `/login`. | PASS | Redirige a `/login` y cambia la barra de navegación |
| 5 | **Protección de rutas `/dashboard/*` post-logout / Botón Atrás (Back Button):** Tras cerrar sesión, navegar directamente por URL a `/dashboard`, `/dashboard/availability`, `/dashboard/clients` o presionar el botón «Atrás» del navegador. | FAIL | **CAQ-4-BUG-01:** La interfaz permite renderizar vistas privadas de dashboard por caché/estado en cliente tras el logout |

---

## Defectos Encontrados

*   **CAQ-4-BUG-01 (Exposición de vistas privadas en cliente tras Logout):** Tras presionar «Salir», la navegación manual hacia rutas privadas (`/dashboard`, `/dashboard/availability`, `/dashboard/clients`) o el uso del botón «Atrás» del navegador continúa renderizando componentes privados en la UI sin forzar la redirección a `/login`.

---

## Trazabilidad

| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-4 | Escenario 1: Inicio de sesión exitoso | — | `.context/testing/exploratory/smoke/evidence/smoke-dashboard-2026-09-07-produccion.png` |
| CAQ-4 | Escenarios 2 y 3: Credenciales inválidas | — | Verificado en UI |
| CAQ-4 | Escenarios 4 y 5: Campos vacíos | — | Verificado en UI |
| CAQ-4 | Escenario 7: Cierre de sesión activa | — | Verificado en UI |
| CAQ-4 | Escenarios 8 y 9: Acceso directo y reingreso post-logout | CAQ-4-BUG-01 | Comportamiento observado de renderizado post-logout |

---

## Sin cobertura

*   **Bloqueo temporal de 5 intentos fallidos (Escenario 12):** No ejecutado en volumen para evitar bloqueo no controlado de IP en entorno de producción compartido.
*   **Concurrencia multisesión entre dispositivos físicos distintos (Escenario 11):** No ejecutado por requerir múltiples dispositivos externos simultáneos en la sesión actual.

---

## Transferencia UI -> API/DB

### Acciones Disparadoras
*   **Submit Formulario `/login`:** Dispara llamada de autenticación (ej. `POST /auth/v1/token?grant_type=password` contra Supabase Auth).
*   **Clic en «Salir» en menú:** Dispara logout en cliente/servidor y redirección a `/login`.
*   **Navegación directa a `/dashboard`:** Dispara solicitudes de página y consultas de datos (`GET /api/appointments`, etc.).

### Datos de Prueba Utilizados
*   **Usuario Profesional:** `usuario.prueba.1@mailinator.com` (referenciado desde perfil seguro / `.env`).
*   **Usuario Inválido Sintético:** `usuario.invalido.qa@mailinator.com`.

### Trazas Observadas
*   **Rutas UI:** `https://cita-ai.vercel.app/login`, `https://cita-ai.vercel.app/dashboard`.
*   **Respuesta de red en login:** 200 en token grant.
