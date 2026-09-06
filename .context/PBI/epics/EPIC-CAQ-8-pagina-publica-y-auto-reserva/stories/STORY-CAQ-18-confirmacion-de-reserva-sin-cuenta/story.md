# Story: Confirmación de reserva sin cuenta

**ID:** CAQ-18
**Epic:** CAQ-8
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como cliente final, quiero confirmar un turno sin crear una cuenta, para reservar con la menor fricción posible.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende de perfil público, disponibilidad y límite freemium. |
| Negociable | Sí | El flujo sin cuenta está definido; validaciones y confirmación visual quedan abiertas. |
| Valiosa | Sí | Reduce la fricción para concretar la reserva. |
| Estimable | Sí | Datos, revalidación y estado final están documentados. |
| Pequeña | Sí | Cubre una única confirmación de turno. |
| Testeable | Sí | Creación, estado, ausencia de cuenta y revalidación son comprobables. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Confirmar una reserva válida

**Given** que el cliente seleccionó un horario disponible
**And** informa su nombre y un correo electrónico válido
**When** confirma la reserva sin crear una cuenta
**Then** el sistema vuelve a comprobar la disponibilidad
**And** crea un único turno con estado `confirmed`
**And** muestra la confirmación de la operación sin requerir aprobación previa

### Escenario 2: Rechazar un nombre vacío
**Given** que el nombre está vacío
**When** el cliente intenta confirmar
**Then** el sistema rechaza la solicitud
**And** muestra «Completa este campo.»

### Escenario 3: Rechazar un correo vacío
**Given** que el correo está vacío
**When** el cliente intenta confirmar
**Then** el sistema rechaza la solicitud
**And** muestra «Completa este campo.»

### Escenario 4: Rechazar un correo inválido
**Given** que el correo tiene formato inválido
**When** el cliente intenta confirmar
**Then** el sistema rechaza la solicitud
**And** muestra «Ingresa un correo electrónico válido.»

### Escenario 5: Rechazar un horario que dejó de estar disponible

**Given** que el horario seleccionado fue ocupado antes de la confirmación
**When** el cliente confirma la reserva
**Then** el sistema no crea el turno
**And** aplica el flujo de conflicto definido en CAQ-19

### Escenario 6: Mostrar el detalle de una reserva confirmada
**Given** que el cliente completó una reserva válida
**When** el sistema confirma el turno
**Then** muestra «Tu turno fue reservado»
**And** presenta profesional, fecha, hora y zona
**And** informa que el enlace de cancelación llegará por correo

### Escenario 7: Evitar duplicados ante un reintento
**Given** que una solicitud ya creó un turno con una clave de idempotencia
**When** se repite dentro de diez minutos con la misma clave
**Then** el sistema devuelve el turno existente
**And** no crea otra reserva

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-18 y decisiones transversales aplicables.

* Nombre y correo usan las mismas reglas de normalización y máximos de CAQ-3. No se solicita contraseña.
* La confirmación visible muestra `Tu turno fue reservado`, profesional, fecha, hora, zona y aviso de que el enlace de cancelación llegará por correo.
* El cliente no necesita aprobación posterior del profesional.
* La UI genera una clave de idempotencia por intento. Repetir durante 10 minutos la misma solicitud con esa clave devuelve el turno existente y no genera otro.
* Reservar para otra persona queda fuera de alcance según la sección 2.5.

## Notas de QA

* Verificar que no se creen cuenta, contraseña ni sesión del cliente.
* Probar reenvío de formulario y conflicto concurrente.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-18.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Reserva con nombre y correo, sin cuenta | `.context/Confluence-corporativo/01-minuta-kickoff.md` · Los dos usuarios del sistema; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 5.1 |
| Revalidación antes de guardar | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · RN-02 |
| Estado confirmado sin aprobación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 9 |
| Rechazo de nombre o correo inválidos | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-18 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-18 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
