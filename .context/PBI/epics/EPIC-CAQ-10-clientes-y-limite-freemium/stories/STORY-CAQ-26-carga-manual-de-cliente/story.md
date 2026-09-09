# Story: Carga manual de un cliente

**ID:** CAQ-26
**Epic:** CAQ-10
**Implementación:** Sin verificar
**Modo de exploración:** Navegador automatizado
**Entorno observado:** producción · 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Bloqueante

## Descripción

Como profesional, quiero cargar un cliente manualmente, para mantener completo mi registro.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | No | Depende del listado y del límite freemium. |
| Negociable | Sí | Datos y límite están definidos; duplicados y validaciones quedan abiertos. |
| Valiosa | Sí | Completa el registro con clientes obtenidos fuera del flujo público. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Cubre el alta de un cliente desde el panel. |
| Testeable | Sí | Alta, límite y aparición en listado son verificables. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Cargar un cliente nuevo dentro del límite

**Given** que el profesional está autenticado y tiene menos de diez clientes únicos
**And** informa nombre y un correo que no está asociado con su cuenta
**When** confirma el alta manual
**Then** el sistema asocia el cliente con el profesional
**And** lo muestra en su listado

### Escenario 2: Rechazar un nombre vacío
**Given** que el nombre está vacío
**When** el profesional intenta crear el cliente
**Then** el sistema rechaza el alta
**And** muestra «Completa este campo.»

### Escenario 3: Rechazar un correo vacío
**Given** que el correo está vacío
**When** el profesional intenta crear el cliente
**Then** el sistema rechaza el alta
**And** muestra «Completa este campo.»

### Escenario 4: Rechazar un correo inválido
**Given** que el correo tiene formato inválido
**When** el profesional intenta crear el cliente
**Then** el sistema rechaza el alta
**And** muestra «Ingresa un correo electrónico válido.»

### Escenario 5: Rechazar el cliente único número once

**Given** que el profesional ya tiene diez clientes únicos
**And** el correo informado corresponde a un cliente nuevo para él
**When** intenta cargarlo
**Then** el sistema rechaza el alta
**And** conserva el listado sin cambios

### Escenario 6: Reutilizar un cliente duplicado
**Given** que el correo normalizado ya existe para el profesional
**When** intenta cargarlo manualmente
**Then** el sistema no crea ni actualiza otro registro
**And** muestra «Este cliente ya existe en tu listado.»
**And** ofrece abrir el cliente existente

### Escenario 7: Actualizar el listado después del alta
**Given** que el profesional crea un cliente válido dentro del límite
**When** el alta finaliza
**Then** el nuevo cliente aparece inmediatamente
**And** el listado conserva su orden alfabético

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-26 y decisiones transversales aplicables.

* `Nuevo Cliente` debe abrir un formulario con nombre y correo bajo las reglas de CAQ-3. La falta de respuesta observada se considera defecto.
* Si el correo ya existe para el profesional, no se crea ni actualiza otro registro. Se muestra `Este cliente ya existe en tu listado.` y se ofrece abrirlo.
* Al alcanzar el límite se muestra `Alcanzaste el límite de 10 clientes del plan gratuito. Tus clientes actuales pueden seguir reservando.`
* Un alta válida aparece inmediatamente en el listado respetando su orden.

## Notas de QA

* Probar el décimo y undécimo cliente y un correo ya relacionado.
* Verificar aislamiento entre profesionales.
* La implementación continúa `Sin verificar`.

## Inspección Shift-Left

**Resultado:** Bloqueante

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-26.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Carga manual desde el panel | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 2.1 |
| Nombre, correo y unicidad | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Tablas |
| Aplicación del límite al alta manual | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 8.1 y 8.2 |
| Aparición inmediata en el listado | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-26 |
| Rechazo de campos inválidos | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-26 |
| La acción «Nuevo Cliente» no produjo un formulario ni un cambio visible | **Observado** — producción, 30/08/2026 y reconfirmado 09/09/2026 mediante Playwright; no se realizó ningún alta. Evidencia: `evidence/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |
| Reconfirmación 09/09/2026: `Nuevo Cliente` sigue sin abrir diálogo, sin errores en consola | **Observado** — producción, 09/09/2026. Evidencia: `evidence/2026-09-09-clientes-nuevo-sin-dialogo-fail.png` |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-26 |

## Contradicciones detectadas

* La documentación y la decisión de Producto exigen un formulario de alta; la acción «Nuevo Cliente» no produjo cambios visibles durante la observación. La discrepancia queda como defecto de implementación.

## Preguntas abiertas

* ¿Por qué la acción «Nuevo Cliente» no abrió el formulario durante la observación de producción?
