# Story: Registro manual de un turno

**ID:** CAQ-14
**Epic:** CAQ-7
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero registrar un turno manualmente, para incorporar reservas coordinadas fuera de la página pública.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende de clientes, disponibilidad y límite freemium. |
| Negociable | Sí | El resultado está definido; el flujo de selección sigue abierto. |
| Valiosa | Sí | Incorpora reservas acordadas por otros canales. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Crea un turno desde el panel. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Registrar un turno manual válido

**Given** que el profesional está autenticado
**And** selecciona un cliente y un horario no reservado
**When** confirma el alta manual
**Then** el sistema crea un único turno asociado con ambos
**And** lo registra con estado `confirmed`

### Escenario 2: Rechazar un horario ocupado

**Given** que ya existe un turno confirmado del profesional en el horario elegido
**When** intenta registrar otro turno manual
**Then** el sistema rechaza la operación
**And** no crea un turno superpuesto

### Escenario 3: Aplicar el límite a un cliente nuevo

**Given** que el alta incorporaría un cliente único nuevo
**And** el profesional ya alcanzó diez clientes únicos
**When** intenta registrar el turno
**Then** el sistema rechaza la incorporación del nuevo cliente
**And** no crea el turno

### Escenario 4: Crear una excepción fuera de la disponibilidad
**Given** que el horario futuro está fuera de la disponibilidad semanal y no está bloqueado ni ocupado
**When** el profesional registra manualmente el turno
**Then** el sistema crea el turno como excepción
**And** lo asocia con el cliente seleccionado

### Escenario 5: Reutilizar un cliente existente
**Given** que el correo normalizado ya pertenece a un cliente del profesional
**When** registra un turno manual con ese correo
**Then** el sistema reutiliza el cliente existente
**And** no cambia silenciosamente su nombre

### Escenario 6: Evitar un turno duplicado por reintento
**Given** que una operación manual ya creó el turno con una clave de idempotencia
**When** se repite la solicitud con la misma clave
**Then** el sistema devuelve el turno existente
**And** no crea otro turno

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-14 y decisiones transversales aplicables.

* Se puede seleccionar un cliente existente o crear uno con nombre y correo bajo las reglas de CAQ-3.
* Se requieren cliente, fecha y hora futura. Se admite una nota opcional de hasta 250 caracteres.
* El profesional puede crear un turno fuera de su disponibilidad semanal como excepción manual.
* No puede crear un turno dentro de un bloqueo ni superpuesto con otro `confirmed`; primero debe eliminar el bloqueo o elegir otro horario.
* Si el correo ya pertenece a un cliente del profesional, se reutiliza el registro y no se cambia su nombre silenciosamente.
* La interfaz deshabilita el envío mientras procesa y el servidor usa una clave de idempotencia. Repetir la misma operación devuelve el turno ya creado.

## Notas de QA

* Probar cliente existente, cliente nuevo décimo y cliente nuevo número once.
* Validar ausencia de duplicados ante envíos repetidos.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-14.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Existencia de turnos cargados manualmente | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · métricas del soft launch; `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · ticket #33 |
| Asociación, estado y ausencia de superposición | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Tablas y reserva; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 5.2 y 9 |
| Aplicación del límite a altas manuales | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.1 |
| Flujo del panel para seleccionar cliente y horario | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-14 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-14 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
