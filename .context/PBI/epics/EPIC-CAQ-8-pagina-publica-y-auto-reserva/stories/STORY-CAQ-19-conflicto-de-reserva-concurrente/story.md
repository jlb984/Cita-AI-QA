# Story: Resolución de un conflicto de reserva concurrente

**ID:** CAQ-19
**Epic:** CAQ-8
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como cliente final, quiero conservar mis datos si otro cliente toma el horario, para elegir otro sin comenzar de nuevo.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Es el caso concurrente de CAQ-18 y depende de la garantía de unicidad. |
| Negociable | Sí | El resultado funcional está claramente documentado. |
| Valiosa | Sí | Evita reservas dobles y repetir la carga de datos. |
| Estimable | Sí | El conflicto, mensaje, conservación y recarga están definidos. |
| Pequeña | Sí | Atiende un único fallo concurrente. |
| Testeable | Sí | Puede ejecutarse con dos sesiones que confirman el mismo horario. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Confirmación ganadora

**Given** que dos clientes seleccionaron el mismo horario disponible
**When** la primera solicitud válida se confirma
**Then** el sistema crea un único turno `confirmed` para ese profesional y horario

### Escenario 2: Rechazar la confirmación perdedora

**Given** que el horario seleccionado dejó de estar disponible
**When** el segundo cliente intenta confirmar
**Then** el sistema rechaza la reserva
**And** informa que otra persona tomó el horario y solicita elegir otro
**And** actualiza la disponibilidad
**And** conserva el nombre y el correo ya ingresados

### Escenario 3: Mantener la unicidad bajo concurrencia

**Given** que llegan simultáneamente varias confirmaciones para el mismo profesional y horario
**When** finalizan las operaciones
**Then** existe como máximo un turno `confirmed` para ese profesional y horario

### Escenario 4: Informar el conflicto sin perder los datos
**Given** que otro cliente acaba de confirmar el mismo slot
**When** el cliente intenta confirmar su reserva
**Then** el sistema anuncia «Este horario acaba de ser reservado por otra persona. Elige otro horario para continuar.»
**And** actualiza inmediatamente la disponibilidad
**And** conserva nombre y correo en la pestaña

### Escenario 5: Garantizar unicidad entre canales
**Given** que una reserva pública y un alta manual compiten por el mismo profesional e instante
**When** ambas solicitudes intentan persistir un turno confirmed
**Then** la base conserva como máximo un turno confirmado
**And** la solicitud perdedora recibe el conflicto verificable

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-19 y decisiones transversales aplicables.

* La base debe garantizar como máximo un turno `confirmed` por profesional e instante. La garantía cubre reserva pública, alta manual y reintentos.
* Mensaje aprobado: `Este horario acaba de ser reservado por otra persona. Elige otro horario para continuar.`
* El mensaje recibe foco o se anuncia mediante una región accesible y la disponibilidad se actualiza inmediatamente.
* Nombre y correo se conservan en la pestaña hasta confirmar otro turno, cerrar la pestaña o abandonar la página pública.
* La validación no transaccional documentada debe reemplazarse o complementarse con una garantía atómica de persistencia.

## Notas de QA

* Ejecutar concurrencia real desde dos sesiones, no una secuencia manual.
* Verificar UI, respuesta API y persistencia.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-19.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Rechazo, mensaje, actualización y conservación de datos | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · RN-02 |
| Garantía de no superposición | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · RN-01 |
| Riesgo de implementación no transaccional | `.context/Confluence-corporativo/04-notas-tecnicas.md` · La reserva; `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · Horarios y disponibilidad |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-19 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
