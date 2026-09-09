# Notas de Evidencia: Cancelación de un turno por el profesional (CAQ-21)

**Fecha:** 2026-09-07
**Feature:** `cancelacion-por-profesional`
**Entorno:** Producción (`https://cita-ai.vercel.app/`)

---

## Observaciones de Ejecución

1. **Flujo de Interacción UI:**
   - La acción de cancelar se ubica en cada fila/tarjeta de *Próximas Citas* en `/dashboard`.
   - Al presionar «Cancelar», la aplicación abre un cuadro de confirmación nativo del navegador con el texto literal: *«¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.»*.
   - No solicita motivo de cancelación en la UI.
   - Si se acepta, el elemento se remueve instantáneamente de la lista en pantalla.

2. **Comportamiento Anómalo Detectado (Falso Éxito):**
   - El retiro en la interfaz es meramente reactivo/optimista.
   - Al realizar una navegación forzada o recargar `/dashboard` sin caché, el turno vuelve a mostrarse en la lista con estado confirmado.
   - Evidencia gráfica de referencia: `.context/PBI/epics/EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-CAQ-21-cancelacion-por-profesional/evidence/2026-09-02-turno-reaparece-tras-recarga-sin-cache.png`.

3. **Validación de Restricción de 2 Horas:**
   - No existe bloqueo en el cliente para citas dentro de la ventana de 2 horas. La acción de cancelar sigue habilitada y despliega el mismo diálogo de confirmación genérico sin mostrar el mensaje de advertencia requerido por la especificación.
