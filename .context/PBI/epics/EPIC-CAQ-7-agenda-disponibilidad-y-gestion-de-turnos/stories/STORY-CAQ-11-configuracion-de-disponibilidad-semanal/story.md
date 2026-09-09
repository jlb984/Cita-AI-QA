# Story: Configuración de disponibilidad semanal

**ID:** CAQ-11
**Epic:** CAQ-7
**Implementación:** Parcial
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Requiere cambios

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

### Escenario 7: Desactivar un día sin cancelar turnos
**Given** que un día tiene turnos confirmados
**When** el profesional desactiva ese día y guarda
**Then** los turnos confirmados se conservan
**And** los horarios futuros de ese día dejan de ofrecerse

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-11 y decisiones transversales aplicables.

* Los bloques se interpretan en la zona horaria del profesional.
* Se permiten bloques contiguos; no se consideran solapados.
* Un bloque no puede cruzar medianoche. Debe dividirse en dos días.
* Mensajes aprobados:
  * Fin no posterior al inicio: `La hora de fin debe ser posterior a la hora de inicio.`
  * Solapamiento: `Este horario se superpone con otro bloque del mismo día.`
  * Cruce de medianoche: `Divide el horario en dos bloques, uno para cada día.`
* El guardado es atómico: reemplaza toda la configuración o conserva íntegramente la anterior y muestra `No pudimos guardar tu disponibilidad. Intenta nuevamente.`
* Desactivar un día equivale a quitar sus intervalos; los turnos `confirmed` no se cancelan y sus slots dejan de ofrecerse (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-11, sesión QA 09/09/2026 pendiente de ratificación).
* Sin tope funcional de intervalos por día en este release (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-11, sesión QA 09/09/2026 pendiente de ratificación).

## Notas de QA

* Probar días sin atención, varios bloques válidos y límites contiguos.
* Comprobar el reemplazo completo y su efecto en el endpoint público.
* Exploración 09/09/2026: guardado válido y rechazo de rango inválido verificados de punta a punta; solapamiento, contigüidad, cruce de medianoche y fallo atómico quedan sin recorrer.

## Inspección Shift-Left

**Resultado:** Requiere cambios (09/09/2026; anterior: Aprobado)

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-11.md`

## Comportamiento observado
| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| La ruta `/dashboard/availability` muestra `Horario Semanal Recurrente` con un interruptor por día, un intervalo `09:00 - 14:00` de lunes a viernes, `No disponible` en domingo y sábado, botón `Añadir intervalo` por día activo y botón `Guardar Cambios`. | `evidence/2026-09-09-horario-semanal.png` | La especificación describe disponibilidad por día con bloques (`.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.1); ningún documento describe este estado visual exacto. |
| Con fin anterior al inicio en martes (09:00–08:00), al guardar muestra `La hora de inicio debe ser anterior a la de fin en Martes` y no persiste nada. | `evidence/2026-09-09-mensaje-rango-invalido.png` | Las decisiones aprueban otro texto para este caso (ver Contradicciones detectadas). |
| Al restaurar el valor válido y guardar, muestra `Horario actualizado correctamente` y la recarga confirma que la configuración persiste (lun–vie 09:00–14:00). | `evidence/2026-09-09-guardado-ok.png` | Coincide con el reemplazo completo de reglas de la especificación (`.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.1). |
| No se recorrieron solapamiento, bloques contiguos, cruce de medianoche ni fallo de guardado. | Sin evidencia: no observado | Los escenarios 3–6 exigen esos recorridos; quedan sin verificar. |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Configuración por día y validaciones de los bloques | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.1 |
| Reemplazo completo al guardar | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.1; `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · Horarios |
| Uso de las reglas para calcular slots | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Los slots |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-11 |
| Configuración semanal observada y mensajes de guardado | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-horario-semanal.png`, `evidence/2026-09-09-mensaje-rango-invalido.png`, `evidence/2026-09-09-guardado-ok.png` |

## Contradicciones detectadas

* Las decisiones de Producto aprueban `La hora de fin debe ser posterior a la hora de inicio.` (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-11); el sistema muestra `La hora de inicio debe ser anterior a la de fin en Martes` (`evidence/2026-09-09-mensaje-rango-invalido.png`). No se elige: o el texto acordado no se implementó, o se cambió sin actualizar la decisión.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional salvo ratificación PO de las dos decisiones de sesión QA 09/09/2026 (día desactivado y sin tope de intervalos).
* Queda abierta solo la pregunta de redacción: ¿vale el texto observado con día incluido, o debe implementarse el texto aprobado para el release 1.1?
