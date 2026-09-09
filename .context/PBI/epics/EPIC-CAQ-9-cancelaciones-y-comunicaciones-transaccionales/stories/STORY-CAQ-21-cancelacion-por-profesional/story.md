# Story: Cancelación de un turno por el profesional

**ID:** CAQ-21
**Epic:** CAQ-9
**Implementación:** Parcial
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Bloqueante

## Descripción

Como profesional, quiero cancelar un turno desde mi panel, para actualizar mi agenda.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede validarse con un turno futuro preparado. |
| Negociable | Sí | Estado y liberación están definidos; el motivo sigue abierto. |
| Valiosa | Sí | Mantiene la agenda y la disponibilidad actualizadas. |
| Estimable | Sí | El flujo, la restricción temporal y los efectos esperados están documentados. |
| Pequeña | Sí | Cubre la cancelación desde el panel. |
| Testeable | Sí | La UI, persistencia, aislamiento y liberación son verificables; la prueba observada detectó una brecha. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Cancelar un turno futuro propio

**Given** que el profesional autenticado tiene un turno futuro `confirmed` asociado con su cuenta
**When** inicia y confirma la cancelación desde su agenda
**Then** el sistema cambia el estado persistido a `cancelled`
**And** retira el turno de las próximas citas
**And** vuelve a ofrecer el horario según las reglas vigentes

### Escenario 2: Impedir cancelar un turno ajeno

**Given** que el turno pertenece a otro profesional
**When** el usuario intenta cancelar ese turno
**Then** el sistema rechaza la operación
**And** no modifica el estado ni la disponibilidad

### Escenario 3: Impedir cancelar un turno pasado

**Given** que el turno asociado con la cuenta está en el pasado
**When** el profesional intenta cancelarlo
**Then** el sistema rechaza la operación
**And** conserva su estado

### Escenario 4: Confirmar la acción en la interfaz observada

**Given** que el profesional selecciona «Cancelar» en una cita futura
**When** la interfaz solicita confirmación
**Then** muestra «¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.»
**And** no exige un motivo

### Escenario 5: Rechazar una cancelación dentro de dos horas
**Given** que faltan menos de dos horas para el inicio del turno
**When** el profesional intenta cancelarlo
**Then** el sistema conserva el turno
**And** muestra «Ya no puedes cancelar este turno desde el panel. Contacta al cliente.»

### Escenario 6: Registrar un motivo opcional válido
**Given** que el profesional cancela un turno dentro de la ventana permitida
**When** informa un motivo de hasta 250 caracteres
**Then** el sistema asocia el motivo con la cancelación
**And** no exige completarlo

### Escenario 7: Conservar la UI ante un fallo persistente
**Given** que la API no logra persistir cancelled y liberar el slot
**When** el profesional confirma la cancelación
**Then** la interfaz no informa éxito
**And** mantiene el turno visible como confirmado

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-21 y decisiones transversales aplicables.

* Aplica la ventana general de 2 horas.
* El motivo es opcional y admite hasta 250 caracteres. No se exigirá hasta que una necesidad posterior lo justifique.
* El diálogo de confirmación conserva el texto observado: `¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.`
* Turno pasado o dentro de la ventana: `Ya no puedes cancelar este turno desde el panel. Contacta al cliente.`
* La autorización del servidor exige que el turno pertenezca al profesional autenticado.
* La API solo responde éxito después de persistir `cancelled` y liberar el slot. La UI no puede retirar el turno definitivamente ante una respuesta fallida.
* El comportamiento observado el 02/09/2026 es un defecto bloqueante. Desarrollo debe investigar la causa y si se intentó enviar un correo; Producto no presume una explicación ni un hecho que la evidencia no confirma.

## Notas de QA

* Repetir la verificación con turno sintético y comprobar UI, API y disponibilidad después de una navegación nueva.
* La persistencia y liberación fallaron en producción el 02/09/2026; no tratar el retiro inmediato de la UI como éxito.
* La implementación se conserva `Parcial`.

## Inspección Shift-Left

**Resultado:** Bloqueante

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-21.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Cancelación desde el panel | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 2.1 y 6.2 |
| Aislamiento por profesional | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Row level security |
| Restricción temporal, estado y liberación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3 |
| Motivo de cancelación | **Pregunta abierta** — `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3 |
| Inicio de la cancelación desde una cita futura y ausencia de campo de motivo | **Observado** — producción, 02/09/2026, cuenta profesional con datos sintéticos. Evidencia: `evidence/2026-09-02-turno-futuro-con-accion-cancelar.png` |
| Retiro inmediato de la cita después de confirmar | **Observado** — producción, 02/09/2026. Evidencia: `evidence/2026-09-02-turno-cancelado-retirado-de-proximas-citas.png` |
| Falta de persistencia y de liberación del horario | **Observado** — producción, 02/09/2026. Evidencia: `evidence/2026-09-02-turno-reaparece-tras-recarga-sin-cache.png`, `evidence/2026-09-02-api-turno-permanece-confirmed.png`, `evidence/2026-09-02-slot-cancelado-no-disponible-en-pagina-publica.png` y `evidence/2026-09-02-api-slot-cancelado-no-liberado.png` |

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| En una inspección previa del mismo día, la ruta `/dashboard` mostró `Próximas Citas` vacía. Esa observación no permitió recorrer la cancelación. | `evidence/2026-09-02-dashboard-sin-turnos-para-cancelar.png` | La especificación funcional indica que el profesional cancela desde su panel (`.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.2). |
| En la sesión autenticada actual, `Próximas Citas` mostró siete turnos futuros sintéticos `Prueba QA 1`; cada uno ofrecía las acciones `Contactar` y `Cancelar`. | `evidence/2026-09-02-turno-futuro-con-accion-cancelar.png` | Coincide con la cancelación desde el panel prevista en la sección 6.2 de la especificación funcional. |
| Al iniciar la cancelación del turno del 3 de septiembre a las 07:00, la aplicación mostró el texto literal “¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.” en un diálogo nativo del navegador y no solicitó motivo. Al aceptar, retiró inmediatamente ese turno de la lista. El diálogo no pudo capturarse porque Playwright bloquea las capturas mientras está abierto; el estado anterior y posterior sí quedó evidenciado. | `evidence/2026-09-02-turno-futuro-con-accion-cancelar.png` y `evidence/2026-09-02-turno-cancelado-retirado-de-proximas-citas.png` | La documentación deja el motivo como `TBD`; no exige confirmación ni define su texto. |
| La cancelación no persistió: una navegación nueva del dashboard volvió a mostrar el mismo turno y `GET /api/appointments` lo devolvió con `status: "confirmed"`. | `evidence/2026-09-02-turno-reaparece-tras-recarga-sin-cache.png` y `evidence/2026-09-02-api-turno-permanece-confirmed.png` | Contradice la sección 6.3, que exige que el turno pase a estado cancelado. |
| El horario tampoco volvió a la oferta pública: para el 3 de septiembre se ofrecieron 09:00, 09:30 y luego 10:30, pero no 10:00, instante UTC del turno que el dashboard presentó como 07:00 en `America/Buenos_Aires`. La respuesta de `GET /api/public/availability` confirmó la misma ausencia. | `evidence/2026-09-02-slot-cancelado-no-disponible-en-pagina-publica.png` y `evidence/2026-09-02-api-slot-cancelado-no-liberado.png` | Contradice la sección 6.3, que exige que el horario vuelva a estar disponible. |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-21 |

## Contradicciones detectadas

* El requisito exige persistir cancelled y liberar el slot antes de informar éxito; la evidencia del 02/09/2026 mostró el turno confirmed y el slot ocupado. La discrepancia queda como defecto bloqueante de implementación.

## Preguntas abiertas

* ¿Cuál es la causa técnica del falso éxito observado y llegó a intentarse el envío del correo de cancelación?
