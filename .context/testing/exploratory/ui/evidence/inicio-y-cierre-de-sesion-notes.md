# Notas de Evidencia: Inicio y cierre de sesión (CAQ-4)

**Fecha:** 2026-09-08
**Feature:** `inicio-y-cierre-de-sesion`
**Entorno:** Producción (`https://cita-ai.vercel.app/`)

---

## Observaciones de Ejecución

1. **Camino Feliz y Validación de Formulario en `/login`:**
   - Carga correcta de la pantalla de Login con título y campos accesibles.
   - El formulario no permite envío con campos vacíos.
   - Credenciales inválidas devuelven un mensaje de error genérico sin exponer detalles del sistema.
   - Login con credenciales válidas redirige exitosamente al Dashboard profesional.

2. **Cierre de Sesión y Protección de Rutas (Hallazgo Crítico):**
   - El botón «Salir» elimina el estado en la barra de navegación y redirige visualmente a `/login`.
   - Sin embargo, al usar el historial de navegación hacia atrás o escribir directamente `/dashboard`, `/dashboard/availability` o `/dashboard/clients`, la UI vuelve a renderizar las pantallas privadas en lugar de forzar la redirección estricta al login.
   - Esto coincide con los hallazgos previos documentados en [.context/infrastructure/environments.md](file:///C:/Users/admin/OneDrive/Desktop/demo-qa/demo%20Clonar%20proyect/mis%20proyectos/Cita-AI-QA/.context/infrastructure/environments.md) y en el PRD del sistema.
