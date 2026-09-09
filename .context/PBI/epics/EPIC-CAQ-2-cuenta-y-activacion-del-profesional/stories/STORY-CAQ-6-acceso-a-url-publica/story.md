# Story: Acceso a la URL pública del profesional

**ID:** CAQ-6
**Epic:** CAQ-2
**Implementación:** Implementada
**Modo de exploración:** Navegador automatizado
**Entorno observado:** Producción · 2026-09-07 / 2026-09-08 / 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero localizar y copiar mi URL pública desde la experiencia autenticada, para compartir mi página de reservas con mis clientes.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede resolverse mostrando el slug ya generado para una cuenta existente, sin modificar el flujo público de reservas. |
| Negociable | Sí | La necesidad de encontrar y compartir la URL está acordada; la ubicación, el control y la confirmación visual son negociables. |
| Valiosa | Sí | Resuelve la consulta principal de los profesionales nuevos y permite comenzar a recibir reservas. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Se concentra en exponer y copiar un dato existente dentro de la experiencia autenticada. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Consulta de la URL desde la experiencia autenticada

**Given** que el profesional tiene una sesión activa y su cuenta posee un slug público
**When** accede a la experiencia autenticada
**Then** puede localizar su URL pública sin intervención de soporte
**And** la URL mostrada corresponde a su propia cuenta

### Escenario 2: Composición de la URL con el dominio vigente

**Given** que la cuenta profesional tiene asignado un slug
**When** el sistema presenta su URL pública
**Then** la compone como `https://cita-ai.vercel.app/` seguida por el slug del profesional

### Escenario 3: Generación del slug a partir del nombre

**Given** que el nombre del profesional no produce una colisión con otro slug
**When** el sistema genera su URL pública
**Then** convierte el nombre a minúsculas y elimina los acentos
**And** reemplaza los espacios por guiones para formar el slug

### Escenario 4: Resolución de una colisión de slug

**Given** que ya existe en la plataforma un slug generado a partir del mismo nombre
**When** el sistema genera la URL del nuevo profesional
**Then** agrega al slug un sufijo numérico incremental que no esté utilizado
**And** la URL resultante es única en toda la plataforma

### Escenario 5: Copia directa de la URL pública

**Given** que el profesional está visualizando su URL pública
**When** ejecuta la acción de copiar
**Then** el sistema copia la URL completa
**And** la URL copiada coincide exactamente con la que se muestra

### Escenario 6: Disponibilidad del enlace durante la activación

**Given** que el profesional inicia el proceso de registro
**When** completa los pasos necesarios para activar su cuenta
**Then** puede localizar su enlace de reservas funcionando antes de que transcurran 5 minutos desde el inicio del registro

### Escenario 7: Mostrar y copiar el enlace desde el dashboard
**Given** que el profesional autenticado tiene un slug público
**When** visualiza la tarjeta permanente «Tu enlace público de reservas»
**Then** el sistema muestra la URL pública completa
**And** ofrece «Copiar enlace» y «Abrir perfil»

### Escenario 8: Confirmar la copia de forma accesible
**Given** que el profesional visualiza su enlace de reservas
**When** selecciona «Copiar enlace»
**Then** el sistema copia la URL completa
**And** anuncia «Enlace copiado» mediante un mensaje accesible

### Escenario 9: Abrir el perfil sin disponibilidad
**Given** que el profesional todavía no configuró disponibilidad
**When** abre su URL pública
**Then** el sistema muestra el perfil
**And** informa «No hay horarios disponibles por el momento.»

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-6 y decisiones transversales aplicables.

* El dashboard muestra una tarjeta permanente denominada `Tu enlace público de reservas`, con la URL completa y acciones `Copiar enlace` y `Abrir perfil`.
* El slug no es editable y no cambia al modificar el nombre durante el release 1.1.
* El dominio canónico del release es `https://cita-ai.vercel.app/`.

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| En `/dashboard`, el sistema muestra una tarjeta destacada con título «Tu enlace público de reservas», subtítulo «Compártelo con tus clientes para que puedan consultar horarios y reservar.», el input con `https://cita-ai.vercel.app/[slug]`, etiqueta de slug (`Slug: jorge-luis-ecosistemas-403`), y botones `Abrir perfil` y `Copiar enlace`. | `evidence/2026-09-07-tarjeta-enlace-publico-dashboard.png` | Coincide con la decisión de Producto para el release 1.1 y resuelve la brecha de localización previa. |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| El slug se genera en minúsculas, sin acentos y con espacios reemplazados por guiones | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.4 |
| Las colisiones se resuelven con un sufijo numérico incremental y el slug es único en toda la plataforma | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.4 |
| El dominio vigente de las páginas públicas es `https://cita-ai.vercel.app/` | `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · La dirección, 21/05/2026 |
| El profesional visualiza la tarjeta de enlace público con botones para abrir y copiar en `/dashboard` | **Observado** — Producción, 2026-09-07. Evidencia: `evidence/2026-09-07-tarjeta-enlace-publico-dashboard.png` |
| Tarjeta re-confirmada con sesión el 09/09/2026 | Sesión 09/09/2026: `.context/testing/exploratory/ui/session-2026-09-09-cuenta-y-activacion.md` |
| Ofrecer una acción directa para copiar la URL completa | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-6 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-6 |

## Contradicciones detectadas

* Ninguna detectada tras la incorporación de la tarjeta visible en el dashboard observada en producción.

## Preguntas abiertas

* Ninguna detectada.
