# Story: Listado de clientes del profesional

**ID:** CAQ-25
**Epic:** CAQ-10
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero consultar mi listado de clientes, para conocer a las personas que reservaron conmigo.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede probarse con clientes y turnos preparados. |
| Negociable | Sí | Alcance y unicidad están definidos; orden e historial quedan abiertos. |
| Valiosa | Sí | Permite reconocer la base de clientes del profesional. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Se limita a la consulta de clientes asociados. |
| Testeable | Sí | Asociación, aislamiento y unicidad por correo son comprobables. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Consultar clientes propios

**Given** que el profesional autenticado tiene turnos asociados con clientes
**When** abre su listado de clientes
**Then** el sistema muestra el nombre y correo de esos clientes
**And** no incluye clientes sin relación con el profesional

### Escenario 2: Unificar un cliente con varios turnos

**Given** que el mismo correo está asociado con varios turnos del profesional
**When** consulta el listado
**Then** el sistema presenta una única entrada para ese correo

### Escenario 3: Aislar clientes de otro profesional

**Given** que un cliente reservó únicamente con otro profesional
**When** el profesional autenticado consulta su listado
**Then** el sistema no expone ese cliente

### Escenario 4: Ordenar el listado de clientes
**Given** que el profesional tiene clientes con nombres repetidos
**When** consulta el listado
**Then** el sistema ordena alfabéticamente por nombre
**And** usa el correo como segundo criterio

### Escenario 5: Buscar clientes
**Given** que el profesional tiene clientes registrados
**When** busca parcialmente por nombre
**Then** el sistema muestra solo las coincidencias de su cuenta

### Escenario 6: Paginar el listado
**Given** que el profesional tiene más de veinte clientes
**When** consulta la página siguiente
**Then** el sistema presenta el bloque siguiente de hasta veinte clientes

### Escenario 7: Excluir el historial del listado
**Given** que un cliente tiene uno o más turnos
**When** el profesional consulta el listado de clientes
**Then** el sistema muestra una única entrada para ese correo normalizado
**And** no incluye el historial de turnos

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-25 y decisiones transversales aplicables.

* El correo se normaliza con trim y minúsculas para determinar unicidad dentro de cada profesional.
* El listado se ordena alfabéticamente por nombre y, ante empate, por correo.
* Pagina de a 20 y permite búsqueda parcial por nombre o correo.
* El historial de turnos queda fuera de CAQ-25 y requiere una Story separada.
* Solo se muestran clientes asociados con el profesional autenticado.

## Notas de QA

* Preparar dos profesionales, correos repetidos y múltiples turnos.
* No asumir historial, filtros ni paginación como parte de esta historia.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-25.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Listado de clientes | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 2.1 |
| Asociación por turnos y unicidad por correo | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Tablas y límite del plan gratuito |
| Nombre y correo del cliente | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 2.2 |
| Historial por cliente | **Pregunta abierta** — aparece como necesidad en entrevistas, pero no forma parte del comportamiento especificado |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-25 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
