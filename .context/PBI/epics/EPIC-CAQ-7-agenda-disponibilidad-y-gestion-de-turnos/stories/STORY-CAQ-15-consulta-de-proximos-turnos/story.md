# Story: Consulta de próximos turnos

**ID:** CAQ-15
**Epic:** CAQ-7
**Implementación:** Parcial
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero consultar mis próximos turnos, para organizar mi jornada.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede validarse con turnos preparados para una cuenta. |
| Negociable | Sí | La consulta está definida; orden, filtros y detalle siguen abiertos. |
| Valiosa | Sí | Permite organizar la jornada del profesional. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Se limita a consultar los próximos turnos. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Consultar próximos turnos propios

**Given** que el profesional tiene una sesión válida y turnos futuros asociados
**When** abre su dashboard
**Then** el sistema presenta únicamente sus próximos turnos
**And** permite distinguir el horario y el estado `confirmed` o `cancelled` de cada uno

### Escenario 2: Mostrar el estado vacío

**Given** que no existen citas próximas visibles para la cuenta
**When** se abre el dashboard
**Then** el sistema muestra «No tienes citas próximas»
**And** muestra «Comparte tu perfil público para empezar a recibir reservas.»

### Escenario 3: Impedir exposición entre profesionales

**Given** que un turno pertenece a otro profesional
**When** el profesional autenticado consulta su agenda
**Then** el sistema no incluye ese turno en la respuesta ni en la interfaz

### Escenario 4: Mostrar y ordenar los próximos turnos
**Given** que el profesional autenticado tiene más de un turno futuro confirmado
**When** consulta «Próximas Citas»
**Then** el sistema muestra nombre y correo del cliente, fecha, hora, zona, estado y acciones
**And** ordena los turnos de menor a mayor fecha

### Escenario 5: Paginar próximos turnos
**Given** que el profesional tiene más de veinte turnos futuros confirmados
**When** consulta la página siguiente
**Then** el sistema presenta el bloque siguiente de hasta veinte turnos

### Escenario 6: Buscar próximos turnos
**Given** que el profesional tiene turnos futuros confirmados
**When** busca por nombre del cliente
**Then** el sistema muestra únicamente las coincidencias de su cuenta

### Escenario 7: Filtrar próximos turnos por fecha
**Given** que el profesional tiene turnos futuros confirmados en distintas fechas
**When** aplica un rango de fechas
**Then** el sistema muestra únicamente los turnos comprendidos en el rango

### Escenario 8: Excluir turnos cancelados
**Given** que el profesional tiene turnos futuros confirmados y cancelados
**When** consulta «Próximas Citas»
**Then** el sistema muestra únicamente los turnos confirmed
**And** no incluye los cancelled

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-15 y decisiones transversales aplicables.

* Requiere sesión y autorización de servidor. Solo devuelve turnos del profesional autenticado.
* `Citas Hoy` cuenta turnos `confirmed` con inicio hoy en la zona del profesional; todo lo contado debe listarse en `Próximas Citas` (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-15, sesión QA 09/09/2026 pendiente de ratificación).
* Cada fila muestra fecha y hora en la zona del profesional con identificador o abreviatura visible (`.context/product-decisions/decisiones-po-proximo-release.md` · 2.2).
* Cada fila muestra nombre y correo del cliente, fecha, hora y zona, estado y acciones disponibles.
* `Próximas Citas` contiene turnos futuros `confirmed`, ordenados de menor a mayor fecha.
* La vista pagina de a 20 turnos y permite filtrar por rango de fechas y buscar por nombre o correo.
* Los turnos `cancelled` quedan fuera de `Próximas Citas`; su historial se incorporará en una Story separada.
* El estado vacío observado se conserva con los textos actuales.

## Notas de QA

* Preparar turnos pasados, futuros, confirmados y cancelados para dos profesionales.
* El estado vacío está observado; la lista poblada y el aislamiento no lo están.
* Exploración 09/09/2026: estado vacío y contradicción de contadores documentados; escenarios con datos quedan sin recorrer (no se crean turnos masivos en producción).

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-15.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Consulta de agenda y próximos turnos | `.context/architecture/prd.md` · Feature 2; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 2.1 |
| Aislamiento por profesional | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Row level security y endpoints |
| Estados de turno | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 9 |
| Presentación exacta de horario y estado | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-15 |
| Textos del estado vacío | **Observado** — producción, 02/09/2026. Evidencia: `evidence/2026-09-02-dashboard-sin-citas.png` |
| Estado vacío y contadores del 09/09/2026 | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-dashboard-proximas-citas.png`, `evidence/2026-09-09-dashboard-contadores.png` |

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| La ruta `/dashboard` renderiza el panel con los indicadores `Citas Hoy`, `Próxima Cita` y la sección `Próximas Citas`. | `evidence/2026-09-02-dashboard-sin-citas.png` | El PRD describe una agenda y consulta de turnos próximos (`.context/architecture/prd.md` · Feature 2), pero no define este estado visual. |
| Cuando no hay citas próximas, se muestra `No tienes citas próximas` y `Comparte tu perfil público para empezar a recibir reservas.` | `evidence/2026-09-02-dashboard-sin-citas.png` | **Nada: ningún documento describe el estado vacío.** |
| No se pudo observar una lista con turnos, sus horarios o estados porque la cuenta disponible no tenía citas y no se crearon datos en producción. | `evidence/2026-09-02-dashboard-sin-citas.png` | La historia exige distinguir horario y estado; queda sin verificar. |
| El 09/09/2026 el dashboard muestra `Citas Hoy` con `1 Programada para hoy` y `Próxima Cita --:--`, mientras `Próximas Citas` muestra `No tienes citas próximas` y `Comparte tu perfil público para empezar a recibir reservas.` | `evidence/2026-09-09-dashboard-contadores.png`, `evidence/2026-09-09-dashboard-proximas-citas.png` | **Observado** — producción, 09/09/2026. Ver Contradicciones detectadas. |
| No se observaron lista poblada, orden, paginado, búsqueda, filtros, estados `confirmed`/`cancelled` ni aislamiento entre profesionales (sin datos para provocarlos y sin crear 20+ turnos en producción). | Sin evidencia: no observado | Los escenarios 1 y 4–8 exigen esos recorridos; quedan sin verificar. |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-15 |

## Contradicciones detectadas

* El mismo dashboard afirma `1 Programada para hoy` en `Citas Hoy` y a la vez `No tienes citas próximas` con `Próxima Cita --:--` (`evidence/2026-09-09-dashboard-contadores.png`, `evidence/2026-09-09-dashboard-proximas-citas.png`). No se elige: o hay una cita de hoy que la lista no muestra, o el contador cuenta algo que no es una cita próxima visible.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional salvo ratificación PO de la regla de conteo (sesión QA 09/09/2026). El formato de zona queda respondido por 2.2.
* Queda abierta como defecto del sistema: ¿qué cuenta `Citas Hoy` que `Próximas Citas` no lista?
