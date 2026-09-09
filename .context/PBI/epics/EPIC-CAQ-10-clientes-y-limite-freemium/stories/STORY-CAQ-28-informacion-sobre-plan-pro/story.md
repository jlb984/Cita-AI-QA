# Story: Información sobre el Plan Pro al alcanzar el límite

**ID:** CAQ-28
**Epic:** CAQ-10
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional que alcanzó el límite, quiero recibir información clara sobre el Plan Pro, para conocer mis opciones.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende de que CAQ-27 detecte el límite. |
| Negociable | Sí | Comunicación y CTA están definidas; oferta comercial queda fuera. |
| Valiosa | Sí | Explica la continuidad del plan y registra intención de conversión. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre correo, aviso y CTA ante un evento concreto. |
| Testeable | Sí | Disparo, persistencia del aviso y ausencia de cobro son verificables. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Informar al alcanzar diez clientes únicos

**Given** que el profesional alcanza diez clientes únicos
**When** el sistema procesa ese evento
**Then** envía al profesional un correo con tono de celebración
**And** muestra en su panel un aviso permanente
**And** explica que sus clientes actuales pueden seguir reservando

### Escenario 2: Solicitar más información

**Given** que el profesional ve el aviso del límite
**When** selecciona «Más información sobre el Plan Pro»
**Then** el sistema registra su interés
**And** no ejecuta un cobro ni confirma una contratación

### Escenario 3: Mantener visible el aviso

**Given** que el profesional ya alcanzó el límite
**When** vuelve a ingresar a su panel
**Then** el aviso sobre el Plan Pro continúa visible

### Escenario 4: Enviar una sola comunicación al alcanzar el límite
**Given** que el profesional alcanza diez clientes únicos por primera vez
**When** el sistema procesa el evento
**Then** envía una sola vez el correo aprobado sobre el Plan Pro
**And** mantiene el aviso en el panel

### Escenario 5: Registrar el interés idempotentemente
**Given** que el profesional visualiza «Más información sobre el Plan Pro»
**When** registra su interés más de una vez
**Then** el sistema conserva un único registro con profesional, fecha UTC, origen y versión
**And** reemplaza la acción por «Interés registrado»
**And** no solicita datos de pago

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-28 y decisiones transversales aplicables.

* Plan Pro no tendrá precio, beneficios, cobro ni contratación en el release 1.1. La acción solo registra interés.
* Al alcanzar diez clientes se envía una sola vez el correo: `¡Tu agenda está creciendo! Alcanzaste los 10 clientes de tu plan gratuito. Tus clientes actuales pueden seguir reservando. Si quieres conocer futuras opciones, registra tu interés en el Plan Pro.`
* El panel muestra permanentemente: `Alcanzaste el límite de 10 clientes. Tus clientes actuales pueden seguir reservando.` y la acción `Más información sobre el Plan Pro`.
* La acción registra `professional_id`, fecha UTC, origen y versión del mensaje. No registra datos de pago.
* La operación es idempotente. Después de ejecutarla, la acción se reemplaza por `Interés registrado`; el aviso permanece visible.
* El correo no vuelve a enviarse si el conteo baja y alcanza nuevamente el límite.

## Notas de QA

* Verificar que el correo se dispare una sola vez por el evento de alcanzar el límite.
* No evaluar precio ni beneficios como criterio hasta que exista una definición.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-28.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Correo y aviso permanente en el panel | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.2 |
| Texto vigente de la llamada a la acción | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · resumen del 03/03/2026 |
| Registro de interés sin plan pago | `.context/Confluence-corporativo/01-minuta-kickoff.md` · El modelo: freemium; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 8.3 |
| Precio, beneficios y cobro | **Pregunta abierta** — el Plan Pro no existe como oferta definida |
| Envío único del correo por evento | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-28 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-28 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
