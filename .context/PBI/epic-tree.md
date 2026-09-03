# Índice del Backlog

**Origen:** `.context/architecture/prd.md`
**Fuentes del backlog:** documentación local y respaldo histórico de Confluence
**Tipo de proyecto:** Brownfield
**Fecha:** 01/09/2026
**Última comprobación contra Jira (CAQ-3):** 2026-09-02

| Epic | Stories | Sin verificar | Estado de sincronización |
| :--- | :---: | :---: | :--- |
| CAQ-2 [Epic] Cuenta y activación del profesional | 4 | 4 | Sincronizado |
| CAQ-7 [Epic] Agenda, disponibilidad y gestión de turnos | 5 | 5 | Sincronizado |
| CAQ-8 [Epic] Página pública y auto-reserva | 4 | 4 | Sincronizado |
| CAQ-9 [Epic] Cancelaciones y comunicaciones transaccionales | 5 | 5 | Sincronizado |
| CAQ-10 [Epic] Clientes y límite freemium | 4 | 4 | Sincronizado |

## Epics identificadas, pendientes de desglosar

* Ninguna: el backlog está desglosado entero.

## Pendiente de subir a Jira

* Ninguna: todo sincronizado.

## Pendiente de verificar contra la aplicación

* CAQ-3 a CAQ-6 y CAQ-11 a CAQ-28 permanecen `Sin verificar` por tratarse de un proyecto Brownfield y existir únicamente un entorno de producción con datos reales.

## Contradicciones detectadas

* La especificación describe `cita.ai`, `uat.cita.ai` y dos bases separadas; `nota-ambientes-y-accesos.md`, más reciente, establece que solo está activa producción en `https://cita-ai.vercel.app/`. Se toma la nota del 21/05/2026 por ser posterior.
* La especificación y las notas técnicas describen el dashboard protegido por middleware; el PRD registra que, después de cerrar sesión, las rutas del dashboard continuaron mostrando sus pantallas. El comportamiento real de autorización permanece sin verificar.
* La especificación exige una contraseña de al menos ocho caracteres, una mayúscula y un número; no hay evidencia actual que confirme que la interfaz y Supabase apliquen exactamente esas tres validaciones.
* La especificación afirma que la URL pública se genera al registrarse, pero soporte y la observación del producto indican que no se muestra en el panel. Se conserva la generación como comportamiento documentado y su exposición como brecha.
* Las notas técnicas antiguas atribuyen todos los correos a Supabase; el hilo del 03/03/2026 establece que los correos de producto migraron a Resend y los de autenticación permanecieron en Supabase. Se toma el hilo más reciente.
* La especificación acepta cualquier duración entera positiva; la interfaz observada ofrece 15, 30, 45, 60, 90 y 120 minutos. No se elige una regla hasta que negocio la confirme.
* La especificación presenta el recordatorio del día anterior como requisito; el hilo del 03/03/2026 y la reunión del 19/05/2026 confirman que fue excluido del lanzamiento y sigue sin implementarse. Se conserva como brecha priorizada.
* La especificación exige impedir superposiciones, pero las notas técnicas documentan una validación no transaccional y soporte registró duplicados. El comportamiento esperado se conserva y la implementación queda sin verificar.
* El resumen automático atribuye ambos casos de turnos duplicados a husos horarios; la transcripción confirma esa causa solo para uno. Se toma la transcripción como fuente original.

## Preguntas abiertas

* ¿Debe bloquearse temporalmente una cuenta después de varios intentos fallidos de inicio de sesión? ¿Después de cuántos intentos?
* ¿El cierre de sesión debe redirigir inmediatamente al login y eliminar cualquier contenido del dashboard visible en el navegador?
* ¿Dónde y mediante qué interacción debe mostrarse la URL pública para cumplir la promesa de activación en menos de cinco minutos?
* ¿Qué sucede con los turnos existentes cuando el profesional bloquea el período que los contiene?
* ¿Cuál es la anticipación máxima para reservar y existe una ventana mínima de cancelación?
* ¿Qué zona horaria rige la agenda y cómo se presenta un turno a clientes ubicados en otros países?
* ¿La duración válida es cualquier entero positivo o únicamente una opción de la interfaz?
* ¿Debe existir el estado `No se presentó`?
* ¿Cómo se representa a quien reserva para otra persona?
* ¿Qué incluye el Plan Pro, cuánto cuesta y cómo se procesa una solicitud de información?
* ¿Cuáles son los objetivos acordados de rendimiento, disponibilidad, concurrencia, accesibilidad y compatibilidad?
* No se recorrió la aplicación en esta fase; solo existe un entorno de producción y el prompt de backlog no autoriza generar datos reales para verificar las Stories.
