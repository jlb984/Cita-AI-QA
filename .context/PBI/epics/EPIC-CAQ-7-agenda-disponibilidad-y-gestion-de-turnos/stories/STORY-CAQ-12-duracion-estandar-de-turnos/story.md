# Story: Duración estándar de los turnos

**ID:** CAQ-12
**Epic:** CAQ-7
**Implementación:** Parcial
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero definir la duración estándar de mis turnos, para generar horarios acordes con mi servicio.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede validarse sobre una disponibilidad ya configurada. |
| Negociable | Sí | El objetivo está definido; el conjunto de valores sigue abierto. |
| Valiosa | Sí | Permite adecuar los horarios ofrecidos al servicio. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Modifica una única regla global del profesional. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Aplicar una duración válida

**Given** que el profesional tiene bloques de disponibilidad configurados
**And** selecciona una duración admitida mayor que cero
**When** guarda la duración estándar
**Then** el sistema aplica esa duración a todos sus turnos
**And** divide cada bloque en horarios consecutivos con esa duración

### Escenario 2: Rechazar una duración no positiva

**Given** que el profesional informa una duración igual o menor que cero
**When** intenta guardar
**Then** el sistema rechaza el valor
**And** conserva la duración anterior

### Escenario 3: No ofrecer un remanente incompleto

**Given** que la duración vigente no divide exactamente un bloque disponible
**When** el sistema calcula sus horarios
**Then** ofrece únicamente turnos completos dentro del bloque

### Escenario 4: Rechazar una duración fuera del catálogo
**Given** que el profesional intenta configurar una duración distinta de 15, 30, 45, 60, 90 o 120 minutos
**When** guarda la configuración
**Then** el sistema rechaza el valor
**And** conserva la duración anterior

### Escenario 5: Preservar los turnos existentes al cambiar la duración
**Given** que el profesional tiene turnos ya creados
**When** cambia la duración estándar
**Then** el sistema aplica el nuevo valor solo a los slots futuros
**And** no modifica la duración ni la hora de los turnos existentes

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-12 y decisiones transversales aplicables.

* Los únicos valores válidos son 15, 30, 45, 60, 90 y 120 minutos. El valor inicial es 60 minutos.
* Un cambio afecta únicamente la generación de slots futuros. Nunca modifica la duración ni la hora de turnos ya creados.
* Si un bloque deja un remanente menor que la duración, el remanente se descarta y no genera un slot incompleto.
* La interfaz observada se adopta como regla del próximo release y reemplaza la regla histórica de cualquier entero positivo.

## Notas de QA

* Probar cada opción observada y bloques con división exacta e inexacta.
* No aprobar valores fuera de la interfaz hasta resolver la contradicción.
* Exploración 09/09/2026: catálogo verificado; cambiar y guardar la duración, remanentes y turnos existentes quedan sin recorrer (cambiarla muta la generación de slots en producción).

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-12.md`

## Comportamiento observado
| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| `Configuración General` ofrece `Duración de la Cita (minutos)` como lista cerrada con `15`, `30`, `45`, `60`, `90` y `120 minutos`; el valor seleccionado es `30 minutos`. Hay botón `Guardar` propio de la sección. | `evidence/2026-09-09-duracion-cita.png` | Coincide con el catálogo adoptado para el release 1.1 (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-12). El valor inicial documentado era 60; el observado es 30 en esta cuenta. |
| No se cambió ni guardó la duración, no se verificó descarte de remanentes ni preservación de turnos existentes. | Sin evidencia: no observado | Los escenarios 1, 3 y 5 exigen esos recorridos; quedan sin verificar. |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Duración positiva, única y usada para dividir la franja | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 4.2 |
| Opciones observadas de 15 a 120 minutos | `.context/architecture/prd.md` · Feature 2 |
| Conjunto definitivo de duraciones permitidas | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-12 |
| Un remanente menor que la duración no genera un horario | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-12 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-12 |
| Catálogo de duraciones y valor seleccionado | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-duracion-cita.png` |

## Contradicciones detectadas

* La especificación histórica admitía cualquier entero positivo y la interfaz observada ofrecía un catálogo cerrado. Producto adopta 15, 30, 45, 60, 90 y 120 minutos para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
