# Story: Configuración de disponibilidad semanal

**ID:** CAQ-11
**Epic:** CAQ-7
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero configurar mi disponibilidad semanal, para ofrecer únicamente los horarios en los que atiendo.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede validarse con una cuenta profesional sin crear reservas. |
| Negociable | Sí | La regla semanal está definida; la interacción de edición queda abierta. |
| Valiosa | Sí | Determina cuándo puede recibir reservas el profesional. |
| Estimable | Sí | Están definidos bloques, validaciones y reemplazo de reglas. |
| Pequeña | Sí | Se limita a administrar la disponibilidad recurrente. |
| Testeable | Sí | Las reglas guardadas y los slots resultantes son observables. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Guardar una disponibilidad semanal válida

**Given** que el profesional está autenticado
**And** define uno o más bloques de atención por día
**And** cada hora de fin es posterior a su hora de inicio
**And** los bloques del mismo día no se solapan
**When** guarda la configuración
**Then** el sistema reemplaza las reglas semanales anteriores por las informadas
**And** calcula la disponibilidad pública únicamente con las reglas vigentes

### Escenario 2: Rechazar un bloque con rango inválido

**Given** que un bloque tiene una hora de fin igual o anterior a la hora de inicio
**When** el profesional intenta guardar
**Then** el sistema rechaza la configuración
**And** conserva las reglas anteriores

### Escenario 3: Rechazar bloques solapados

**Given** que dos bloques del mismo día se superponen
**When** el profesional intenta guardar
**Then** el sistema rechaza la configuración
**And** conserva las reglas anteriores

### Escenario 4: Aceptar bloques contiguos
**Given** que un bloque comienza exactamente cuando termina otro del mismo día
**When** el profesional guarda la disponibilidad
**Then** el sistema acepta ambos bloques
**And** no los considera solapados

### Escenario 5: Rechazar un bloque que cruza medianoche
**Given** que un bloque comienza un día y termina al día siguiente
**When** el profesional intenta guardarlo
**Then** el sistema rechaza el bloque
**And** muestra «Divide el horario en dos bloques, uno para cada día.»

### Escenario 6: Conservar la configuración ante un fallo
**Given** que existe una disponibilidad guardada
**When** falla el reemplazo de la configuración semanal
**Then** el sistema conserva íntegramente la configuración anterior
**And** muestra «No pudimos guardar tu disponibilidad. Intenta nuevamente.»

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-11 y decisiones transversales aplicables.

* Los bloques se interpretan en la zona horaria del profesional.
* Se permiten bloques contiguos; no se consideran solapados.
* Un bloque no puede cruzar medianoche. Debe dividirse en dos días.
* Mensajes aprobados:
  * Fin no posterior al inicio: `La hora de fin debe ser posterior a la hora de inicio.`
  * Solapamiento: `Este horario se superpone con otro bloque del mismo día.`
  * Cruce de medianoche: `Divide el horario en dos bloques, uno para cada día.`
* El guardado es atómico: reemplaza toda la configuración o conserva íntegramente la anterior y muestra `No pudimos guardar tu disponibilidad. Intenta nuevamente.`

## Notas de QA

* Probar días sin atención, varios bloques válidos y límites contiguos.
* Comprobar el reemplazo completo y su efecto en el endpoint público.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-11.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Configuración por día y validaciones de los bloques | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.1 |
| Reemplazo completo al guardar | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.1; `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · Horarios |
| Uso de las reglas para calcular slots | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Los slots |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-11 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
