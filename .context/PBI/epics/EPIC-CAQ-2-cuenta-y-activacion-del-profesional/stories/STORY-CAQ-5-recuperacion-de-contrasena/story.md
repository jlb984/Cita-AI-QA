# Story: Recuperación de contraseña del profesional

**ID:** CAQ-5
**Epic:** CAQ-2
**Implementación:** Parcial
**Modo de exploración:** Navegador automatizado
**Entorno observado:** Producción · 2026-09-07 / 2026-09-08 / 09/09/2026
**Estado de sincronización:** PENDIENTE DE SUBIR A JIRA
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero solicitar un enlace de recuperación mediante mi correo electrónico y establecer una nueva contraseña, para volver a acceder sin revelar si mi correo está registrado.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede validarse con cuentas existentes y no existentes sin depender de las funciones de agenda o reservas. |
| Negociable | Sí | El objetivo de recuperación segura y no enumerativa está definido; la presentación y los controles adicionales siguen siendo negociables. |
| Valiosa | Sí | Permite recuperar el acceso sin intervención manual y protege la existencia de las cuentas. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | Sí | Comprende un único flujo de recuperación compuesto por solicitud, recepción del enlace y cambio de contraseña. |
| Testeable | Sí | La respuesta no enumerativa, la vigencia de una hora, el uso único y el cambio de contraseña pueden comprobarse con buzones y cuentas de prueba. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Solicitud para un correo registrado

**Given** que existe una cuenta profesional asociada al correo informado
**When** el profesional solicita recuperar su contraseña
**Then** el sistema muestra el mensaje «Si el email existe en nuestro sistema, recibirás un enlace para recuperar tu contraseña»
**And** envía al correo registrado un enlace con un token de un solo uso

### Escenario 2: Solicitud para un correo no registrado

**Given** que no existe una cuenta asociada al correo informado
**When** el usuario solicita recuperar la contraseña
**Then** el sistema muestra el mensaje «Si el email existe en nuestro sistema, recibirás un enlace para recuperar tu contraseña»
**And** la respuesta visible no revela que el correo no está registrado

### Escenario 3: Solicitud con correo vacío
**Given** que el usuario deja vacío el correo
**When** intenta solicitar la recuperación
**Then** el sistema no procesa la solicitud
**And** muestra «Ingresa un correo electrónico válido.»

### Escenario 4: Solicitud con formato de correo inválido
**Given** que el usuario informa un correo con formato inválido
**When** intenta solicitar la recuperación
**Then** el sistema no procesa la solicitud
**And** muestra «Ingresa un correo electrónico válido.»

### Escenario 5: Cambio de contraseña con un token vigente

**Given** que el profesional abre un enlace de recuperación no utilizado antes de que transcurra una hora desde su emisión
**When** establece una nueva contraseña
**Then** el sistema actualiza la contraseña de la cuenta
**And** permite recuperar el acceso con la nueva contraseña

### Escenario 6: Intento con un token vencido

**Given** que transcurrió una hora o más desde la emisión del token de recuperación
**When** el profesional intenta utilizar el enlace
**Then** el sistema rechaza el cambio de contraseña
**And** no modifica la contraseña de la cuenta

### Escenario 7: Reutilización de un token

**Given** que el token de recuperación ya fue utilizado para cambiar la contraseña
**When** se intenta utilizar nuevamente el mismo enlace
**Then** el sistema rechaza el cambio de contraseña
**And** no modifica la contraseña de la cuenta

### Escenario 8: Invalidar enlaces anteriores
**Given** que existe un enlace de recuperación todavía vigente
**When** el profesional solicita un nuevo enlace
**Then** el sistema invalida todos los enlaces anteriores de esa cuenta
**And** solo el enlace más reciente puede utilizarse

### Escenario 9: Rechazar la reutilización de contraseña
**Given** que el profesional abre un enlace de recuperación válido
**When** informa la misma contraseña que tenía antes
**Then** el sistema rechaza el cambio
**And** muestra la política de contraseña vigente

### Escenario 10: Limitar solicitudes repetidas por correo
**Given** que un correo ya realizó cinco solicitudes durante una hora
**When** se realiza una solicitud adicional
**Then** el sistema mantiene la respuesta no enumerativa
**And** no envía otro correo

### Escenario 11: Limitar solicitudes repetidas por IP
**Given** que una IP ya realizó cinco solicitudes durante una hora
**When** se realiza una solicitud adicional desde esa IP
**Then** el sistema mantiene la respuesta no enumerativa
**And** no envía otro correo

### Escenario 12: Finalizar una recuperación exitosa
**Given** que el profesional establece una contraseña nueva válida
**When** el sistema confirma el cambio
**Then** invalida las sesiones existentes
**And** redirige a /login
**And** no inicia una sesión automáticamente

## Decisiones de Producto incorporadas

Fuente vigente: `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-5 y decisiones transversales aplicables.

* La nueva contraseña usa exactamente la política de CAQ-3 y no puede ser igual a la contraseña anterior.
* Una nueva solicitud invalida todos los enlaces de recuperación anteriores de esa cuenta.
* Se permiten hasta 5 solicitudes por correo y por IP en una hora. Al alcanzar el límite se conserva la respuesta no enumerativa y no se envía otro correo.
* Mensajes aprobados:
  * Solicitud aceptada o correo inexistente: `Si el email existe en nuestro sistema, recibirás un enlace para recuperar tu contraseña.`
  * Correo inválido: `Ingresa un correo electrónico válido.`
  * Token inválido, vencido o usado: `Este enlace ya no es válido. Solicita uno nuevo.`
* Después del cambio exitoso se invalidan las sesiones existentes y se redirige al login. No se inicia sesión automáticamente.

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| En la pantalla `/login`, la interfaz muestra el enlace `¿Olvidaste tu contraseña?` junto al campo de Contraseña para iniciar el flujo de restablecimiento. | `evidence/2026-09-07-enlace-olvidaste-contrasena.png` | Coincide con la sección 3.3 de la especificación funcional (`.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md`). |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| La recuperación se solicita mediante el correo electrónico | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.3 |
| La respuesta visible es idéntica exista o no la cuenta | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.3; `.context/architecture/prd.md` · Feature 1 y Requisitos No Funcionales |
| Mensaje «Si el email existe en nuestro sistema, recibirás un enlace para recuperar tu contraseña» | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.3 |
| Enlace `¿Olvidaste tu contraseña?` visible en el formulario de login | **Observado** — Producción, 2026-09-07. Evidencia: `evidence/2026-09-07-enlace-olvidaste-contrasena.png` |
| Formulario de recupero accesible con sesión (sin envío, sin captura) | Sesión 09/09/2026: `.context/testing/exploratory/ui/session-2026-09-09-cuenta-y-activacion.md` |
| El enlace contiene un token de un solo uso que vence a la hora | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.3; `.context/Confluence-corporativo/04-notas-tecnicas.md` · Auth |
| Los correos de recuperación pertenecen a autenticación y permanecen en Supabase | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · correo del 28/02/2026 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-5 |

## Contradicciones detectadas

* Ninguna detectada en la entrada del flujo en login.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
