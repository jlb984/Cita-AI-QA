# Índice del Backlog

**Origen:** `.context/architecture/prd.md`
**Fuentes del backlog:** documentación local y respaldo histórico de Confluence
**Decisiones vigentes para el próximo release:** `.context/product-decisions/decisiones-po-proximo-release.md`
**Jira canónico:** `https://jlb984.atlassian.net/` · Project Key `CAQ`
**GitHub canónico:** `https://github.com/jlb984/Cita-AI-QA`
**Adaptación local:** completada y documentada en `.context/PBI/reconciliacion-backlog-caq.md`
**Tipo de proyecto:** Brownfield
**Fecha:** 04/09/2026

| Epic | Stories | Refinadas | Inspeccionadas | Sin verificar | Estado de sincronización |
| :--- | :---: | :---: | :---: | :---: | :--- |
| CAQ-2 [Epic] Cuenta y activación del profesional | 4 | 4 | 4 | 0 | SINCRONIZADO Y VERIFICADO EN JIRA (CAQ) |
| CAQ-7 [Epic] Agenda, disponibilidad y gestión de turnos | 5 | 5 | 5 | 5 | SINCRONIZADO Y VERIFICADO EN JIRA (CAQ) |
| CAQ-8 [Epic] Página pública y auto-reserva | 4 | 4 | 4 | 4 | SINCRONIZADO Y VERIFICADO EN JIRA (CAQ) |
| CAQ-9 [Epic] Cancelaciones y comunicaciones transaccionales | 5 | 5 | 5 | 4 | SINCRONIZADO Y VERIFICADO EN JIRA (CAQ) |
| CAQ-10 [Epic] Clientes y límite freemium | 4 | 4 | 4 | 4 | SINCRONIZADO Y VERIFICADO EN JIRA (CAQ) |

## Epics identificadas, pendientes de desglosar

* Ninguna: el backlog está desglosado entero.

## Pendiente de subir a Jira

* Ninguna. Las 5 Epics y 22 Stories adaptadas se sincronizaron y verificaron contra Jira
  `CAQ` el 06/09/2026.

## Estado de refinamiento

**Última corrida:** 04/09/2026 · alcance: todas

| Resultado | Cantidad |
| :--- | ---: |
| Stories reprocesadas | 22 |
| Stories refinadas | 22 |
| Escenarios Gherkin vigentes | 157 |
| Escenarios netos agregados en esta corrida | 71 |
| Stories con preguntas funcionales abiertas | 0 |

Las decisiones vigentes de `.context/product-decisions/decisiones-po-proximo-release.md` se incorporaron
como reglas explícitas y escenarios verificables. Las preguntas que permanecen en cuatro
Stories son de diagnóstico técnico sobre discrepancias ya observadas, no decisiones
funcionales pendientes.

**Discrepancias contra Jira antes de la sincronización:** ninguna modificación funcional
más reciente que el material local. Se conservaron las claves reales de `CAQ`.

## Estado de Shift-Left Testing

**Última corrida:** 04/09/2026 · alcance: todas

| Resultado | Cantidad |
| :--- | ---: |
| Stories inspeccionadas | 22 |
| Aprobadas | 18 |
| Requiere cambios | 0 |
| Bloqueantes | 4 |
| Hallazgos abiertos | 4 |

Las decisiones de Producto resolvieron las 69 observaciones de requisitos de la corrida
anterior. Permanecen cuatro discrepancias contra comportamiento observado: protección después
del logout (`CAQ-4`), exposición de la URL pública (`CAQ-6`), cancelación no persistida
(`CAQ-21`) y alta manual de cliente sin respuesta visible (`CAQ-26`).

Los cinco planes de prueba por Epic fueron regenerados con la escala de riesgo vigente, los
objetivos no funcionales aprobados y la necesidad documentada de un entorno QA aislado. La
ejecución mutante continúa bloqueada mientras el mapa real solo confirme Producción y no existan
seed, teardown ni correo sandbox disponibles.

**Sincronización Jira:** completada y verificada el 06/09/2026. Las 27 descripciones se
actualizaron; cada Story conserva exactamente un comentario Shift-Left vigente y uno de
decisiones de Producto, sin duplicados.

## Pendiente de verificar contra la aplicación

* Las Stories de la Epic CAQ-2 (`CAQ-3`, `CAQ-4`, `CAQ-5`, `CAQ-6`) y `CAQ-21` cuentan con verificación/documentación de evidencia observada en producción. Las demás Stories continúan `Sin verificar`.
* CAQ-3 y CAQ-5 fueron documentadas con punto de entrada verificado en la interfaz pública (`/login`).
* CAQ-4 fue documentada y verificada parcialmente: el flujo de login/dashboard funciona, pero el logout no bloquea el renderizado en cliente al navegar directamente o usar el historial.
* CAQ-6 fue verificada como Implementada: la tarjeta «Tu enlace público de reservas» se visualiza y opera correctamente en `/dashboard`.
* CAQ-21 fue verificada parcialmente en producción con un turno sintético: la UI inicia la cancelación, pero el cambio no persiste ni libera el horario.

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
* La especificación funcional exige que cancelar cambie el turno a `cancelled` y libere el horario; en CAQ-21 la UI retiró temporalmente el turno, pero una navegación nueva lo mostró otra vez, el endpoint autenticado lo mantuvo `confirmed` y la disponibilidad pública no liberó el slot.
* Se importó material de `BJHB`, otra ejecución sobre la misma aplicación. El 06/09/2026 se
  comparó contra `CAQ`, se evitaron duplicados y se adaptó el contenido a claves reales. El
  detalle queda en `.context/PBI/reconciliacion-backlog-caq.md`; `BJHB` no debe usarse como
  destino Jira de este repositorio.

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
