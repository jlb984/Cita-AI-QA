# Story: Bloqueo de períodos puntuales

**ID:** CAQ-13
**Epic:** CAQ-7
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero bloquear períodos puntuales, para evitar reservas cuando no estoy disponible.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede comprobarse sobre una agenda configurada. |
| Negociable | Sí | El bloqueo está definido; el tratamiento de turnos existentes está abierto. |
| Valiosa | Sí | Evita reservas durante ausencias excepcionales. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre alta, consulta y eliminación de una excepción de agenda. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Crear un bloqueo válido

**Given** que el profesional está autenticado
**And** informa un inicio anterior al fin y, opcionalmente, un motivo
**When** crea el bloqueo
**Then** el sistema lo incorpora a su listado
**And** deja de ofrecer para nuevas reservas los horarios incluidos

### Escenario 2: Rechazar un rango inválido

**Given** que el fin del bloqueo es igual o anterior al inicio
**When** el profesional intenta crearlo
**Then** el sistema rechaza la operación
**And** no altera la disponibilidad pública

### Escenario 3: Eliminar un bloqueo

**Given** que existe un bloqueo del profesional
**When** lo elimina
**Then** el sistema lo retira del listado
**And** vuelve a calcular los horarios futuros que ya no estén bloqueados ni reservados

### Escenario 4: Advertir sobre turnos incluidos en un bloqueo
**Given** que el período elegido contiene turnos confirmados
**When** el profesional intenta guardar el bloqueo
**Then** el sistema informa cuántos turnos quedan dentro
**And** advierte que continuarán confirmados hasta que se cancelen manualmente

### Escenario 5: Unificar bloqueos superpuestos
**Given** que el profesional registra dos bloqueos superpuestos
**When** el sistema calcula la disponibilidad
**Then** evalúa la unión de ambos intervalos
**And** no ofrece slots dentro de esa unión

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-13 y decisiones transversales aplicables.

* El bloqueo se guarda y muestra en la zona del profesional; la API usa UTC.
* No se permite crear un bloqueo cuyo fin sea pasado ni cuyo fin sea igual o anterior al inicio.
* Se permiten bloqueos superpuestos; para disponibilidad se evalúa la unión de los intervalos.
* Un bloqueo puede cruzar medianoche porque utiliza fechas e instantes completos.
* Los turnos ya confirmados permanecen confirmados. Antes de guardar se muestra cuántos quedan dentro del bloqueo y se advierte que deben cancelarse manualmente si corresponde.
* El bloqueo impide únicamente nuevas reservas. Eliminarlo recalcula los slots que no estén ocupados por turnos.

## Notas de QA

* Probar bloqueos parciales, de día completo y superpuestos entre sí.
* No ejecutar en producción con turnos reales.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-13.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Rango, motivo opcional y exclusión de horarios | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.3 |
| Consulta, creación y eliminación de bloqueos | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Endpoints |
| No alterar silenciosamente turnos existentes | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-13 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-13 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
