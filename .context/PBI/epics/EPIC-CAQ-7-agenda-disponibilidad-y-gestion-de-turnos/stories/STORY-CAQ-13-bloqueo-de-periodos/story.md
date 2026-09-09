# Story: Bloqueo de períodos puntuales

**ID:** CAQ-13
**Epic:** CAQ-7
**Implementación:** Parcial
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Requiere cambios

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

### Escenario 6: Rechazar un bloqueo con fin en el pasado
**Given** que el fin del bloqueo es anterior al momento actual
**When** el profesional intenta crearlo
**Then** el sistema rechaza la operación
**And** no altera la disponibilidad pública

### Escenario 7: Rechazar un motivo mayor a 250 caracteres
**Given** que el motivo supera los 250 caracteres
**When** el profesional intenta crear el bloqueo
**Then** el sistema rechaza la operación
**And** no altera la disponibilidad pública

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13 y decisiones transversales aplicables.

* El bloqueo se guarda y muestra en la zona del profesional; la API usa UTC.
* No se permite crear un bloqueo cuyo fin sea pasado ni cuyo fin sea igual o anterior al inicio.
* Se permiten bloqueos superpuestos; para disponibilidad se evalúa la unión de los intervalos.
* Un bloqueo puede cruzar medianoche porque utiliza fechas e instantes completos.
* Los turnos ya confirmados permanecen confirmados. Antes de guardar se muestra cuántos quedan dentro del bloqueo y se advierte que deben cancelarse manualmente si corresponde.
* El bloqueo impide únicamente nuevas reservas. Eliminarlo recalcula los slots que no estén ocupados por turnos.
* El motivo admite hasta 250 caracteres; excederlo rechaza el guardado sin persistir (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13, sesión QA 09/09/2026 pendiente de ratificación; texto del mensaje pendiente de observación).

## Notas de QA

* Probar bloqueos parciales, de día completo y superpuestos entre sí.
* No ejecutar en producción con turnos reales.
* Exploración 09/09/2026: alta válida y eliminación verificadas de punta a punta con un bloqueo sintético futuro (creado y eliminado en la misma sesión); rango inválido, superpuestos, aviso sobre turnos confirmados y cruce de medianoche quedan sin recorrer.

## Inspección Shift-Left

**Resultado:** Requiere cambios (09/09/2026; anterior: Aprobado)

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-13.md`

## Comportamiento observado
| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| `Bloqueos de Tiempo` pide `Fecha Inicio`, `Hora Inicio` (09:00 por defecto), `Fecha Fin`, `Hora Fin` (17:00 por defecto) y `Motivo (Opcional)` con ejemplo `Ej: Vacaciones, Cita médica`; la fecha exige formato de calendario (el texto libre es rechazado). Sin bloqueos muestra `No hay bloqueos activos`. | `evidence/2026-09-09-bloqueos.png` | Coincide con rango y motivo opcional de la especificación (`.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.3). |
| Un bloqueo 24/12/2026 09:00–17:00 con motivo `QA sintético` se incorpora al listado como `QA sintético` + `24/12/26, 9:00 a. m. - 24/12/26, 5:00 p. m.` | `evidence/2026-09-09-bloqueo-creado.png` | Coincide con el escenario 1 y con que el bloqueo puede cruzar medianoche por usar instantes completos (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13). |
| Eliminar pide confirmación con `¿Estás seguro de eliminar este bloqueo?`; al aceptar, el bloqueo sale del listado y vuelve `No hay bloqueos activos`. | Sin captura del diálogo: se transcribe el texto literal observado | Coincide con el escenario 3. Ningún documento describe el texto de la confirmación. |
| No se probaron rango inválido, superpuestos, aviso sobre turnos confirmados ni cruce de medianoche. | Sin evidencia: no observado | Los escenarios 2, 4 y 5 exigen esos recorridos; quedan sin verificar. |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Rango, motivo opcional y exclusión de horarios | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.3 |
| Consulta, creación y eliminación de bloqueos | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Endpoints |
| No alterar silenciosamente turnos existentes | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-13 |
| Formulario, alta y eliminación de un bloqueo sintético | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-bloqueos.png`, `evidence/2026-09-09-bloqueo-creado.png` |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional salvo ratificación PO del tope de motivo (sesión QA 09/09/2026) y el texto exacto de su mensaje, pendiente de observación.
