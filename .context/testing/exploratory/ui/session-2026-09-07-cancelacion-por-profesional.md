# Sesión Exploratoria UI: Cancelación de un turno por el profesional (CAQ-21)

**Fecha:** 2026-09-07
**Entorno:** Producción (`https://cita-ai.vercel.app/`)
**Duración:** 40 minutos
**Modo de ejecución:** Manual / Asistido (con autorización de mutación sobre la cuenta de prueba del perfil)
**Misión:** Evaluar el flujo de cancelación de citas desde el panel del profesional (`/dashboard`), contrastando el comportamiento de la UI frente a la especificación de CAQ-21 (diálogo de confirmación, persistencia real tras recarga, rechazo de cancelación, ventana límite < 2h y robustez ante interacción de usuario).

---

## Escenarios Probados

| # | Escenario | Resultado | Evidencia |
| :--- | :--- | :--- | :--- |
| 1 | **Flujo normal de cancelación y verificación de persistencia tras recarga:** El profesional hace clic en «Cancelar» en una cita futura sintética en `/dashboard`, confirma en el diálogo nativo/modal y se recarga la página sin caché (F5 / `Ctrl+Shift+R`) para verificar si la cita permanece cancelada o reaparece. | FAIL | `.context/PBI/epics/EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-CAQ-21-cancelacion-por-profesional/evidence/2026-09-02-turno-reaparece-tras-recarga-sin-cache.png` (Reaparece en UI; falso éxito inmediato) |
| 2 | **Rechazo / Cancelación en diálogo de confirmación:** El profesional hace clic en «Cancelar» en una cita de *Próximas Citas*, pero presiona «Cancelar» en el cuadro de confirmación. La cita permanece visible e intacta en el dashboard sin alteración de estado. | PASS | Verificado en flujo UI (no altera la lista ni dispara mutación) |
| 3 | **Cancelación de turno dentro de la ventana restringida (< 2 horas):** El profesional intenta cancelar un turno que inicia en menos de 2 horas. La UI debería rechazarlo mostrando el mensaje de restricción: *«Ya no puedes cancelar este turno desde el panel. Contacta al cliente.»* (Escenario 5 Gherkin). | FAIL | La UI no aplica la regla de 2 horas en cliente y muestra el diálogo estándar de cancelación |
| 4 | **Robustez ante doble clic / clics rápidos (Super User):** Clic múltiple en el botón «Cancelar» antes de que se abra o responda el cuadro de confirmación. La UI no se bloquea ni genera estados corruptos o peticiones duplicadas no controladas. | PASS | Diálogo nativo/modal bloquea la ejecución de eventos posteriores en el hilo |
| 5 | **Comportamiento y adaptabilidad responsive (375px):** Visualización de la lista de *Próximas Citas* en viewport reducido. Los botones de acción («Contactar», «Cancelar») y datos del cliente se muestran accesibles sin cortes ni desbordamientos destructivos. | PASS | La interfaz adapta la tabla/tarjetas de citas adecuadamente al ancho de pantalla |

---

## Defectos Encontrados

*   **CAQ-21-BUG-01 (Falso éxito de cancelación y falta de persistencia):** Al confirmar la cancelación de una cita en `/dashboard`, la UI retira visualmente el elemento del DOM de forma optimista, pero la mutación no persiste en backend/DB ni libera el slot; al recargar la página (`/dashboard`), el turno vuelve a renderizarse como confirmado.
*   **CAQ-21-BUG-02 (Falta de validación de la ventana de 2 horas en UI):** La interfaz no evalúa la diferencia temporal contra la hora actual ni muestra el mensaje requerido por negocio (*«Ya no puedes cancelar este turno desde el panel. Contacta al cliente.»*) para citas que inician dentro de las 2 horas.

---

## Trazabilidad

| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Escenario 1: Cancelar un turno futuro propio y persistencia | CAQ-21-BUG-01 | `.context/PBI/epics/EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-CAQ-21-cancelacion-por-profesional/evidence/2026-09-02-turno-reaparece-tras-recarga-sin-cache.png` |
| CAQ-21 | Escenario 4: Confirmar acción en interfaz y rechazar | — | Verificado en UI |
| CAQ-21 | Escenario 5: Rechazar cancelación dentro de 2 horas | CAQ-21-BUG-02 | Verificado en UI (falta mensaje y bloqueo de ventana) |
| CAQ-21 | Robustez y concurrencia UI (Super User) | — | Comportamiento UI controlado |
| CAQ-21 | Responsive design en lista de citas | — | Visualización correcta en 375px |

---

## Sin cobertura

*   **Cancelación de turnos ajenos desde UI:** No es posible forzar IDs ajenos directamente desde la interfaz sin interceptar llamadas de red o usar capa API (se deriva a Capa 2 API).
*   **Verificación directa en base de datos:** La comprobación de tablas Supabase se relega a la sesión de Capa 3 DB.
*   **Disparo y recepción real de correos de aviso (CAQ-22):** No se comprueba bandeja de entrada externa en esta sesión para evitar spam o dependencias de servicios externos en UI pura.

---

## Transferencia UI -> API/DB

### Acciones Disparadoras
*   **Clic en «Cancelar» -> Confirmar diálogo:** Dispara llamada HTTP a la API de actualización/cancelación de cita (ej. `PATCH /api/appointments/[id]` o `POST /api/appointments/[id]/cancel`).
*   **Recarga de página (`/dashboard`):** Dispara `GET /api/appointments` para poblar el listado de *Próximas Citas*.

### Datos de Prueba Utilizados
*   **Usuario autenticado:** Cuenta profesional de prueba activa en perfil (`usuario.prueba.1@mailinator.com`).
*   **Turno de prueba:** Cita futura sintética asignada al profesional (`Prueba QA 1`).

### Trazas Observadas
*   **Ruta UI:** `https://cita-ai.vercel.app/dashboard`
*   **Estado observado:** Respuesta optimista en frontend; discrepancia en persistencia tras recarga (`confirmed`).
