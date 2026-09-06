# Story: Recordatorio del día anterior

**ID:** CAQ-24
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como cliente final, quiero recibir un recordatorio el día anterior, para reducir el riesgo de olvidar el turno.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede procesar turnos ya existentes. |
| Negociable | Sí | El objetivo está definido; momento, zona y reintentos quedan abiertos. |
| Valiosa | Sí | Reduce ausencias por olvido. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre un recordatorio automático previo. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Recordar un turno confirmado del día siguiente

**Given** que existe un turno `confirmed` que ocurre al día siguiente según la zona horaria acordada
**When** se ejecuta automáticamente el proceso programado
**Then** el sistema envía al cliente un correo de recordatorio antes del turno

### Escenario 2: Excluir un turno cancelado

**Given** que un turno del día siguiente está `cancelled`
**When** se ejecuta el proceso de recordatorios
**Then** el sistema no envía un recordatorio por ese turno

### Escenario 3: Evitar recordatorios duplicados

**Given** que ya se envió el recordatorio de un turno
**When** el proceso vuelve a evaluar el mismo turno
**Then** no envía un segundo recordatorio

### Escenario 4: Omitir un recordatorio para una reserva reciente
**Given** que el turno se crea con menos de veinticuatro horas de anticipación
**When** el scheduler procesa los recordatorios
**Then** el sistema no genera un recordatorio para ese turno

### Escenario 5: Incluir el enlace solo mientras permite cancelar
**Given** que el scheduler genera el recordatorio de un turno confirmado
**When** compone el contenido
**Then** incluye profesional, fecha, hora y zona
**And** incluye el enlace de cancelación únicamente si la ventana todavía lo permite

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-24 y decisiones transversales aplicables.

**Política común de correos de producto**

* Resend es el proveedor para bienvenida, reserva, cancelación y recordatorio. Supabase se conserva para autenticación y recuperación.
* Cada evento funcional tiene un identificador idempotente para evitar correos duplicados.
* Ante fallo se realizan tres reintentos: después de 1 minuto, 10 minutos y 1 hora.
* Agotados los reintentos, el evento queda `failed`, se registra en monitoreo y se alerta al equipo. No se revierte una cuenta, reserva o cancelación ya persistida.
* La interfaz informa éxito de la operación principal y, cuando el usuario autenticado sea el actor, advierte `La operación se completó, pero no pudimos enviar el correo.`

**Decisión específica de CAQ-24**

* El recordatorio se incorpora al release 1.1.
* Se envía al cliente exactamente 24 horas antes del turno `confirmed`. El scheduler opera en UTC y el contenido usa la zona del profesional.
* Un turno creado con menos de 24 horas de anticipación no recibe recordatorio.
* Incluye profesional, fecha, hora, zona y enlace de cancelación si todavía está dentro de la ventana permitida.
* Turnos `cancelled` y `no_show` no generan recordatorios.
* El identificador del turno y el tipo `reminder-24h` forman la clave de deduplicación. Se aplica la política común de tres reintentos.

## Notas de QA

* Controlar el reloj para probar límites de fecha, cancelaciones y reejecuciones.
* La idempotencia es una hipótesis pendiente.
* La implementación continúa `Sin verificar`; las fuentes indican que quedó fuera del lanzamiento.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-24.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Recordatorio del día anterior al cliente | `.context/Confluence-corporativo/01-minuta-kickoff.md` · Lo técnico; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 7 |
| Brecha confirmada y necesidad de ejecución programada | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · correo del 28/02/2026 |
| Demanda posterior al lanzamiento | `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · Recordatorios |
| Exclusión de turnos cancelados y automatización | **Hipótesis técnica** — se deducen del objetivo y de la necesidad de un proceso programado |
| Evitar un segundo recordatorio | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-24 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-24 |

## Contradicciones detectadas

* La especificación inicial incluía el recordatorio y una decisión posterior lo había excluido del primer lanzamiento. Producto lo incorpora expresamente al release 1.1 con envío exactamente 24 horas antes.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
