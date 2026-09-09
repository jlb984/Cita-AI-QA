# Story: Límite de diez clientes únicos del plan gratuito

**ID:** CAQ-27
**Epic:** CAQ-10
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como responsable del producto, quiero limitar el plan gratuito a diez clientes únicos, para validar la intención de conversión.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Atraviesa la reserva pública y el alta manual. |
| Negociable | Sí | Límite, sujeto y excepciones están definidos. |
| Valiosa | Sí | Implementa la validación de intención de conversión. |
| Estimable | Sí | El umbral y los dos puntos de control están documentados. |
| Pequeña | Sí | Aplica una regla transversal acotada. |
| Testeable | Sí | Puede probarse en límites 9, 10 y 11 y con clientes existentes. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Permitir al décimo cliente único

**Given** que el profesional tiene nueve clientes únicos identificados por correo
**When** un cliente nuevo realiza una reserva válida
**Then** el sistema permite crear el turno
**And** contabiliza diez clientes únicos para ese profesional

### Escenario 2: Permitir nuevas reservas a un cliente existente

**Given** que el profesional ya tiene diez clientes únicos
**And** el correo pertenece a uno de ellos
**When** ese cliente realiza otra reserva válida
**Then** el sistema permite crear el turno sin límite de cantidad

### Escenario 3: Rechazar al undécimo cliente público

**Given** que el profesional ya tiene diez clientes únicos
**When** un cliente nuevo intenta reservar
**Then** el sistema rechaza la reserva
**And** le indica que contacte directamente al profesional

### Escenario 4: Rechazar al undécimo cliente manual

**Given** que el profesional ya tiene diez clientes únicos
**When** intenta cargar manualmente un correo nuevo
**Then** el sistema rechaza el alta

### Escenario 5: Contar un cliente creado manualmente
**Given** que un correo normalizado todavía no pertenece a los clientes del profesional
**When** se crea manualmente ese cliente
**Then** el sistema incrementa en uno el conteo de clientes únicos

### Escenario 6: Contar un cliente por su primer turno
**Given** que un correo normalizado todavía no pertenece a los clientes del profesional
**When** ese cliente confirma su primer turno
**Then** el sistema incrementa en uno el conteo de clientes únicos

### Escenario 7: Conservar el conteo al cancelar
**Given** que un cliente ya fue contado y su turno queda cancelado
**When** el sistema recalcula el límite del plan gratuito
**Then** el cliente continúa contando
**And** sus reservas futuras siguen permitidas

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-27 y decisiones transversales aplicables.

* El conteo usa el correo normalizado por profesional.
* Un cliente comienza a contar cuando se crea manualmente o cuando obtiene su primer turno `confirmed`.
* Cancelar turnos no reduce el conteo. El release 1.1 no incluye eliminación de clientes; una futura baja deberá definir su efecto antes de implementarse.
* Los clientes existentes pueden reservar turnos ilimitados.
* Mensaje público para el cliente número once: `Este profesional alcanzó el límite de nuevos clientes. Contacta directamente al profesional para coordinar tu turno.`
* El bloqueo se aplica de forma consistente a reserva pública y alta manual.

## Notas de QA

* Probar correos con diferencias de mayúsculas y espacios sin asumir normalización.
* Verificar que el conteo sea independiente por profesional.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-27.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Límite, unicidad por correo y continuidad de clientes existentes | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.1 |
| Bloqueo de reserva y alta manual | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.2 |
| Mensaje al cliente nuevo bloqueado | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.2 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-27 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
