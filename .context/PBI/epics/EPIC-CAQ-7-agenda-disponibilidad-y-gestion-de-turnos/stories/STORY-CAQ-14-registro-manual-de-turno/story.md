# Story: Registro manual de un turno

**ID:** CAQ-14
**Epic:** CAQ-7
**Implementación:** No encontrada
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Bloqueante

## Descripción

Como profesional, quiero registrar un turno manualmente, para incorporar reservas coordinadas fuera de la página pública.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende de clientes, disponibilidad y límite freemium. |
| Negociable | Sí | El resultado está definido; el flujo de selección sigue abierto. |
| Valiosa | Sí | Incorpora reservas acordadas por otros canales. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Crea un turno desde el panel. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Registrar un turno manual válido

**Given** que el profesional está autenticado
**And** selecciona un cliente y un horario no reservado
**When** confirma el alta manual
**Then** el sistema crea un único turno asociado con ambos
**And** lo registra con estado `confirmed`

### Escenario 2: Rechazar un horario ocupado

**Given** que ya existe un turno confirmado del profesional en el horario elegido
**When** intenta registrar otro turno manual
**Then** el sistema rechaza la operación
**And** no crea un turno superpuesto

### Escenario 3: Aplicar el límite a un cliente nuevo

**Given** que el alta incorporaría un cliente único nuevo
**And** el profesional ya alcanzó diez clientes únicos
**When** intenta registrar el turno
**Then** el sistema rechaza la incorporación del nuevo cliente
**And** no crea el turno

### Escenario 4: Crear una excepción fuera de la disponibilidad
**Given** que el horario futuro está fuera de la disponibilidad semanal y no está bloqueado ni ocupado
**When** el profesional registra manualmente el turno
**Then** el sistema crea el turno como excepción
**And** lo asocia con el cliente seleccionado

### Escenario 5: Reutilizar un cliente existente
**Given** que el correo normalizado ya pertenece a un cliente del profesional
**When** registra un turno manual con ese correo
**Then** el sistema reutiliza el cliente existente
**And** no cambia silenciosamente su nombre

### Escenario 6: Evitar un turno duplicado por reintento
**Given** que una operación manual ya creó el turno con una clave de idempotencia
**When** se repite la solicitud con la misma clave
**Then** el sistema devuelve el turno existente
**And** no crea otro turno

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-14 y decisiones transversales aplicables.

* Se puede seleccionar un cliente existente o crear uno con nombre y correo bajo las reglas de CAQ-3.
* Se requieren cliente, fecha y hora futura. Se admite una nota opcional de hasta 250 caracteres.
* El profesional puede crear un turno fuera de su disponibilidad semanal como excepción manual.
* No puede crear un turno dentro de un bloqueo ni superpuesto con otro `confirmed`; primero debe eliminar el bloqueo o elegir otro horario.
* Si el correo ya pertenece a un cliente del profesional, se reutiliza el registro y no se cambia su nombre silenciosamente.
* La interfaz deshabilita el envío mientras procesa y el servidor usa una clave de idempotencia. Repetir la misma operación devuelve el turno ya creado.

## Notas de QA

* Probar cliente existente, cliente nuevo décimo y cliente nuevo número once.
* Validar ausencia de duplicados ante envíos repetidos.
* Exploración 09/09/2026: el flujo de alta manual no se encontró en el panel (ver Comportamiento observado); ningún escenario pudo recorrerse.

## Inspección Shift-Left

**Resultado:** Bloqueante (09/09/2026; anterior: Aprobado)

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-14.md`

## Comportamiento observado
| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| El panel solo expone las rutas `/dashboard`, `/dashboard/availability` y `/dashboard/clients`; ninguna muestra acción de alta de turno. `Mis Clientes` lista 2 clientes (`Prueba QA 1`, `QA Sintetico 20260907`) con botones `Nuevo Cliente` y `Contactar`; `Contactar` no abre diálogo ni navega. | `evidence/2026-09-09-clientes-sin-alta-turno.png` | Las decisiones describen un flujo del panel para seleccionar cliente y horario (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-14) que no se encontró donde se buscó. |
| Flujo buscado en `/dashboard`, `/dashboard/availability` y `/dashboard/clients` el 09/09/2026 con sesión profesional: no existe botón ni ruta de registro manual de turno. | `evidence/2026-09-09-clientes-sin-alta-turno.png` | **Nada: ningún documento alternativo indica otra ubicación del flujo.** |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Existencia de turnos cargados manualmente | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · métricas del soft launch; `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · ticket #33 |
| Asociación, estado y ausencia de superposición | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Tablas y reserva; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 5.2 y 9 |
| Aplicación del límite a altas manuales | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.1 |
| Flujo del panel para seleccionar cliente y horario | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-14 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-14 |
| Ausencia del flujo en las rutas del panel | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-clientes-sin-alta-turno.png` |

## Contradicciones detectadas

* Las decisiones de Producto describen un flujo del panel para alta manual (`.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-14); el panel observado no expone esa acción en ninguna de sus tres rutas (`evidence/2026-09-09-clientes-sin-alta-turno.png`). No se elige: o el flujo vive en otra ruta no enlazada, o no está construido.
* Inspección 09/09/2026: el registro manual no figura en `.context/architecture/prd.md` · Feature 2 (lista disponibilidad, duración, bloqueos, consulta y cancelación). La regla llega del hilo de alcance y soporte sin contraste contra el PRD.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional. Queda abierta: ¿dónde está el registro manual de turnos, o es funcionalidad no construida?
* Inspección 09/09/2026: ¿el registro manual es alcance vigente aunque el PRD no lo liste?
