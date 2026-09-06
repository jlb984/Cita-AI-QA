# Story: Correos de confirmación de la reserva

**ID:** CAQ-23
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como participante de una reserva, quiero recibir su confirmación por correo, para conservar los datos del turno.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende de una reserva confirmada. |
| Negociable | Sí | Destinatarios y enlace están definidos; detalle y fallos siguen abiertos. |
| Valiosa | Sí | Deja constancia del turno para ambas partes. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre los correos disparados por una reserva. |
| Testeable | Sí | Envíos, destinatarios y enlace pueden verificarse con buzones controlados. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Confirmar por correo al cliente

**Given** que se creó un turno `confirmed`
**When** el sistema procesa la nueva reserva
**Then** envía mediante Resend un correo al cliente con el detalle del turno
**And** incluye su enlace único de cancelación

### Escenario 2: Avisar al profesional

**Given** que se creó un turno `confirmed`
**When** el sistema procesa la nueva reserva
**Then** envía mediante Resend un aviso al profesional

### Escenario 3: Falla de entrega después de crear el turno

**Given** que el turno ya fue creado
**When** falla el envío de uno de los correos
**Then** el sistema conserva el turno `confirmed`
**And** no informa que la reserva completa falló

### Escenario 4: Incluir el contenido para cada destinatario
**Given** que una reserva pública quedó confirmada
**When** el sistema genera los correos
**Then** el cliente recibe profesional, fecha, hora, zona, estado y enlace de cancelación
**And** el profesional recibe cliente, correo, fecha, hora, zona y origen «Reserva pública»

### Escenario 5: Reintentar sin revertir la reserva
**Given** que el turno ya está confirmado y falla una entrega
**When** se procesa la política de correo
**Then** el sistema conserva la reserva
**And** reintenta después de 1 minuto, 10 minutos y 1 hora
**And** deduplica cada envío por su evento funcional

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-23 y decisiones transversales aplicables.

**Política común de correos de producto**

* Resend es el proveedor para bienvenida, reserva, cancelación y recordatorio. Supabase se conserva para autenticación y recuperación.
* Cada evento funcional tiene un identificador idempotente para evitar correos duplicados.
* Ante fallo se realizan tres reintentos: después de 1 minuto, 10 minutos y 1 hora.
* Agotados los reintentos, el evento queda `failed`, se registra en monitoreo y se alerta al equipo. No se revierte una cuenta, reserva o cancelación ya persistida.
* La interfaz informa éxito de la operación principal y, cuando el usuario autenticado sea el actor, advierte `La operación se completó, pero no pudimos enviar el correo.`

**Decisión específica de CAQ-23**

* El cliente recibe nombre del profesional, fecha, hora, zona, estado y enlace de cancelación.
* El profesional recibe nombre y correo del cliente, fecha, hora, zona y origen `Reserva pública`.
* El turno se confirma antes de generar los correos. Una falla de entrega no revierte la reserva.
* Ambos envíos aplican la política común de reintentos y deduplicación.

## Notas de QA

* Validar ambos buzones, datos del turno, enlace único y ausencia de credenciales.
* El escenario de falla es una hipótesis pendiente de ratificación.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-23.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Destinatarios, detalle y enlace de cancelación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 7 |
| Proveedor vigente Resend | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · resumen del 03/03/2026 |
| Independencia entre creación y entrega | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-23 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-23 |

## Contradicciones detectadas

* Las notas antiguas atribuían los correos a Supabase; la decisión posterior y Producto establecen Resend para correos de producto y Supabase para autenticación.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
