# Story: Consulta pública de horarios disponibles

**ID:** CAQ-16
**Epic:** CAQ-8
**Implementación:** Sin verificar
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como cliente final, quiero consultar los horarios disponibles, para elegir una opción conveniente.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | La consulta puede probarse sin confirmar una reserva. |
| Negociable | Sí | Las exclusiones están definidas; navegación y zona horaria quedan abiertas. |
| Valiosa | Sí | Permite al cliente elegir un horario realmente reservable. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre la consulta semanal y selección inicial. |
| Testeable | Sí | Los slots pueden contrastarse con reglas, reservas y bloqueos preparados. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Consultar una semana con horarios disponibles

**Given** que el profesional tiene disponibilidad vigente
**When** el cliente abre una semana de su calendario público
**Then** el sistema muestra los horarios futuros que no estén reservados ni bloqueados
**And** permite seleccionar uno para iniciar la reserva

### Escenario 2: Excluir horarios pasados
**Given** que una franja contiene horarios pasados
**When** el cliente consulta la semana
**Then** el sistema no ofrece esos horarios como seleccionables

### Escenario 3: Excluir horarios ocupados
**Given** que una franja contiene horarios con turnos confirmados
**When** el cliente consulta la semana
**Then** el sistema no ofrece esos horarios como seleccionables

### Escenario 4: Excluir horarios bloqueados
**Given** que una franja contiene horarios bloqueados
**When** el cliente consulta la semana
**Then** el sistema no ofrece esos horarios como seleccionables

### Escenario 5: Semana sin disponibilidad

**Given** que ningún horario de la semana cumple las reglas de disponibilidad
**When** el cliente consulta esa semana
**Then** el sistema no presenta horarios seleccionables

### Escenario 6: Mostrar un horario dentro de la ventana reservable
**Given** que un slot comienza al menos dos horas después y no más de noventa días en el futuro
**When** el cliente consulta la disponibilidad
**Then** el sistema muestra el slot
**And** identifica la zona horaria del profesional

### Escenario 7: Excluir un horario posterior a noventa días
**Given** que un slot comienza más de noventa días en el futuro
**When** el cliente consulta la disponibilidad
**Then** el sistema no muestra el slot

### Escenario 8: Recuperar una consulta fallida
**Given** que el servicio de disponibilidad no responde correctamente
**When** el cliente consulta los horarios
**Then** el sistema muestra «No pudimos cargar los horarios. Intenta nuevamente.»
**And** ofrece una acción de reintento

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-16 y decisiones transversales aplicables.

* Fechas y horarios se presentan en la zona del profesional con una etiqueta visible.
* Solo se muestran slots que respeten la ventana de 2 horas a 90 días.
* Una semana sin slots muestra `No hay horarios disponibles esta semana. Prueba con otra fecha.`
* Un fallo al consultar muestra `No pudimos cargar los horarios. Intenta nuevamente.` y una acción de reintento.

## Notas de QA

* Preparar slots pasados, confirmados, cancelados y bloqueados.
* Verificar límites de día y semana en la zona horaria finalmente acordada.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-16.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Vista semanal y selección de horario | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 5.1 |
| Exclusión de pasado | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · RN-03 |
| Cálculo mediante reglas, turnos y bloqueos | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Los slots |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-16 |
| Slots del día observados (09:00–13:30, sin 10:00) | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-publica-slots-hoy-pass.png` |

## Contradicciones detectadas

* La especificación (RN-03) y 2.3 exigen no ofrecer pasado ni reservar dentro de las 2 horas previas; el 09/09/2026 (~14:36 hora local) se ofrecían slots 09:00–13:30, todos pasados. No se elige: o la zona del profesional no es la supuesta, o el corte no se aplica. Sesión: `.context/testing/exploratory/ui/session-2026-09-09-pagina-publica-reserva.md`.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional salvo: ¿cuál es la zona del profesional de prueba y qué corte aplica a los slots del día?
