# Story: Acceso a la página pública del profesional

**ID:** CAQ-17
**Epic:** CAQ-8
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como cliente final, quiero acceder a la página pública de un profesional, para identificar con quién voy a reservar.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede probarse con un perfil y slug existentes. |
| Negociable | Sí | Acceso y aislamiento están definidos; el contenido del perfil queda abierto. |
| Valiosa | Sí | Identifica al profesional antes de reservar. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Se limita a resolver y presentar una página pública. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Abrir una página pública válida

**Given** que existe un profesional con un slug único
**When** el cliente abre `https://cita-ai.vercel.app/{slug}` sin iniciar sesión
**Then** el sistema presenta únicamente el perfil correspondiente al slug
**And** permite continuar a la consulta de disponibilidad

### Escenario 2: Aislar perfiles por slug

**Given** que existen dos profesionales con slugs diferentes
**When** el cliente abre la URL de uno de ellos
**Then** la página no expone datos privados ni datos del otro profesional

### Escenario 3: Consultar un slug inexistente

**Given** que el slug solicitado no corresponde a un profesional
**When** el cliente abre la URL pública
**Then** el sistema no presenta el perfil de ningún profesional

### Escenario 4: Limitar los datos públicos
**Given** que existe un perfil público válido
**When** un visitante abre su página
**Then** el sistema muestra únicamente nombre visible, zona horaria, duración estándar y slots
**And** no expone correo, identificadores internos, configuración privada ni datos de clientes

### Escenario 5: Mostrar un perfil sin disponibilidad
**Given** que el perfil existe y no tiene disponibilidad configurada
**When** un visitante abre la página pública
**Then** el sistema conserva la identidad visible del profesional
**And** muestra «No hay horarios disponibles por el momento.»

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-17 y decisiones transversales aplicables.

* Los únicos datos públicos son nombre visible, zona horaria, duración estándar y slots disponibles.
* Correo del profesional, identificadores internos, configuración privada y datos de otros clientes nunca son públicos.
* Un slug inexistente responde HTTP 404 y muestra `No encontramos este perfil.`
* Un perfil válido sin disponibilidad mantiene la identidad del profesional y muestra `No hay horarios disponibles por el momento.`

## Notas de QA

* Probar acceso anónimo, dos perfiles distintos y un slug inexistente.
* Revisar que la respuesta pública no incluya campos privados.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-17.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Acceso público sin cuenta y por slug | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 2.2 y 3.4 |
| Consulta pública del perfil | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Row level security y endpoints públicos |
| Dominio vigente | `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · La dirección |
| Contenido visual exacto del perfil | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-17 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-17 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
