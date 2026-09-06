# Story: Aviso de cancelación a la contraparte

**ID:** CAQ-22
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como parte de un turno cancelado, quiero recibir un aviso, para conocer el cambio.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende de una cancelación exitosa de CAQ-20 o CAQ-21. |
| Negociable | Sí | Destinatario y contenido mínimo están definidos. |
| Valiosa | Sí | Informa oportunamente el cambio a la contraparte. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre la notificación derivada de cancelar. |
| Testeable | Sí | Destinatario, contenido y ausencia de envío al actor son comprobables. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Avisar al profesional cuando cancela el cliente

**Given** que el cliente cancela exitosamente un turno
**When** el sistema procesa la cancelación
**Then** envía al profesional un aviso que identifica que canceló el cliente
**And** no envía ese aviso al cliente que inició la acción

### Escenario 2: Avisar al cliente cuando cancela el profesional

**Given** que el profesional cancela exitosamente un turno
**When** el sistema procesa la cancelación
**Then** envía al cliente un aviso que identifica que canceló el profesional
**And** no envía ese aviso al profesional que inició la acción

### Escenario 3: Falla de entrega posterior a la cancelación

**Given** que el turno ya cambió a `cancelled`
**When** falla el envío del aviso
**Then** el sistema conserva el estado cancelado

### Escenario 4: Incluir el detalle de la cancelación
**Given** que una cancelación quedó persistida
**When** el sistema genera el aviso a la contraparte
**Then** incluye quién canceló, profesional, fecha, hora, zona y estado «Cancelado»
**And** no envía el aviso al mismo actor que canceló

### Escenario 5: Reintentar una entrega fallida
**Given** que la cancelación ya quedó persistida y falla el primer envío
**When** se procesa la política de correo
**Then** el sistema reintenta después de 1 minuto, 10 minutos y 1 hora
**And** no revierte la cancelación

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-22 y decisiones transversales aplicables.

**Política común de correos de producto**

* Resend es el proveedor para bienvenida, reserva, cancelación y recordatorio. Supabase se conserva para autenticación y recuperación.
* Cada evento funcional tiene un identificador idempotente para evitar correos duplicados.
* Ante fallo se realizan tres reintentos: después de 1 minuto, 10 minutos y 1 hora.
* Agotados los reintentos, el evento queda `failed`, se registra en monitoreo y se alerta al equipo. No se revierte una cuenta, reserva o cancelación ya persistida.
* La interfaz informa éxito de la operación principal y, cuando el usuario autenticado sea el actor, advierte `La operación se completó, pero no pudimos enviar el correo.`

**Decisión específica de CAQ-22**

* Si cancela el cliente se notifica solo al profesional; si cancela el profesional se notifica solo al cliente.
* El aviso incluye quién canceló, nombre del profesional, fecha, hora, zona horaria y estado `Cancelado`.
* El correo se genera únicamente después de persistir la cancelación.
* Una falla de entrega no revierte la cancelación y usa la política común de reintentos.

## Notas de QA

* Usar buzones sintéticos y comprobar destinatario, ausencia de duplicados y contenido.
* El escenario de falla conserva el turno por hipótesis y requiere ratificación.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-22.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Destinatario y contenido del aviso | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 6.3 y 7 |
| Existencia actual del correo de cancelación | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · correo del 28/02/2026 |
| Manejo de una falla de correo posterior a cancelar | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-22 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-22 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
