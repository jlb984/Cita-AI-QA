# Story: Cancelación por el cliente mediante enlace

**ID:** CAQ-20
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como cliente final, quiero cancelar mediante el enlace de mi correo, para liberar el horario sin crear una cuenta.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende del correo de confirmación y su enlace único. |
| Negociable | Sí | El objetivo y efectos están definidos; la ventana de cancelación queda abierta. |
| Valiosa | Sí | Permite liberar el horario sin crear una cuenta. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre una cancelación iniciada por el cliente. |
| Testeable | Sí | Estado, autorización por token y liberación pueden comprobarse. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Cancelar un turno futuro mediante enlace

**Given** que el cliente posee el enlace único de un turno futuro `confirmed`
**When** abre el enlace y confirma la cancelación sin iniciar sesión
**Then** el sistema cambia el turno a `cancelled`
**And** vuelve a ofrecer el horario según las reglas vigentes

### Escenario 2: Rechazar la cancelación de un turno pasado

**Given** que el enlace corresponde a un turno pasado
**When** el cliente intenta cancelarlo
**Then** el sistema rechaza la operación
**And** no modifica el turno

### Escenario 3: Reutilizar el enlace de un turno ya cancelado

**Given** que el turno del enlace ya está `cancelled`
**When** el cliente vuelve a usarlo
**Then** el sistema no ejecuta una segunda cancelación
**And** no crea ni modifica otros turnos

### Escenario 4: Rechazar una cancelación dentro de dos horas
**Given** que faltan menos de dos horas para el inicio del turno
**When** el cliente abre el enlace e intenta cancelar
**Then** el sistema conserva el turno
**And** muestra «Ya no puedes cancelar este turno desde el enlace. Contacta al profesional.»

### Escenario 5: Rechazar un token inválido
**Given** que el enlace contiene un token inválido
**When** el cliente intenta cancelar
**Then** el sistema no modifica el turno
**And** muestra «El enlace de cancelación no es válido.»

### Escenario 6: Persistir y liberar antes de informar éxito
**Given** que el token y la ventana de cancelación son válidos
**When** el cliente confirma la cancelación
**Then** el sistema persiste cancelled y libera el slot en una única operación
**And** solo después informa el éxito

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-20 y decisiones transversales aplicables.

* El enlace contiene un token aleatorio de al menos 256 bits; en la base se conserva solamente su hash.
* El token es válido desde la confirmación hasta el límite de cancelación de 2 horas antes del turno.
* Reenviar la confirmación reutiliza el mismo enlace mientras siga vigente. No existe regeneración autónoma del token en este release.
* Un turno ya cancelado muestra `Este turno ya fue cancelado.` sin repetir efectos.
* Token inválido: `El enlace de cancelación no es válido.`
* Turno pasado o dentro de la ventana restringida: `Ya no puedes cancelar este turno desde el enlace. Contacta al profesional.`
* Una cancelación válida persiste `cancelled` y libera el slot en una única operación antes de informar éxito.

## Notas de QA

* Probar token válido, alterado, de otro turno y ya utilizado.
* Verificar persistencia y disponibilidad pública después de cancelar.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-20.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Enlace único sin cuenta | `.context/Confluence-corporativo/01-minuta-kickoff.md` · Los dos usuarios del sistema; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.1 |
| Restricción temporal, estado y liberación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3 |
| Riesgo del endpoint público | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Endpoints |
| No repetir efectos al reutilizar un enlace | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-20 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-20 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
