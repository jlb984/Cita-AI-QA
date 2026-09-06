# Story: Registro del profesional

**ID:** CAQ-3
**Epic:** CAQ-2
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Aprobado

## Descripción

Como profesional, quiero registrarme con mi nombre completo, correo electrónico y contraseña, para crear mi cuenta, iniciar sesión y comenzar a configurar mi agenda.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede desarrollarse y validarse como puerta de entrada al producto, aunque necesita los servicios de autenticación, generación de URL y correo. |
| Negociable | Sí | El valor y las reglas están definidos; la solución técnica y la presentación de las validaciones siguen siendo negociables. |
| Valiosa | Sí | Permite que el profesional cree su cuenta y comience la activación necesaria para recibir reservas. |
| Estimable | Sí | Los campos, límites, reglas y resultados esperados están documentados; las preguntas abiertas afectan detalles de validación y fallos parciales, no impiden una estimación inicial. |
| Pequeña | No | Incluye cinco resultados observables: crear la cuenta, iniciar sesión, generar la URL, redirigir y enviar el correo. Si no cabe en una iteración, dividir en registro y sesión, generación de URL y bienvenida, conservando esta historia como flujo integrador. |
| Testeable | Sí | Las entradas, los límites y los efectos del registro pueden comprobarse; la política real aplicada por autenticación permanece sin verificar. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Registro exitoso del profesional

**Given** que el correo electrónico no está registrado y el profesional informa un nombre completo de hasta 100 caracteres, un correo válido de hasta 254 caracteres y una contraseña de al menos 8 caracteres con una mayúscula y un número

**When** envía el formulario de registro

**Then** el sistema crea la cuenta e inicia la sesión automáticamente

**And** genera una URL pública única para el profesional

**And** redirige al asistente de configuración inicial

**And** envía el correo de bienvenida al profesional

### Escenario 2: Rechazo del nombre obligatorio vacío
**Given** que el profesional deja vacío el nombre completo
**When** intenta enviar el formulario de registro
**Then** el sistema rechaza el registro
**And** muestra «Completa este campo.»
**And** no crea la cuenta

### Escenario 3: Rechazo del correo obligatorio vacío
**Given** que el profesional deja vacío el correo electrónico
**When** intenta enviar el formulario de registro
**Then** el sistema rechaza el registro
**And** muestra «Completa este campo.»
**And** no crea la cuenta

### Escenario 4: Rechazo de la contraseña obligatoria vacía
**Given** que el profesional deja vacía la contraseña
**When** intenta enviar el formulario de registro
**Then** el sistema rechaza el registro
**And** muestra «Completa este campo.»
**And** no crea la cuenta

### Escenario 5: Rechazo de un nombre que supera el máximo

**Given** que el profesional informa un nombre completo de más de 100 caracteres

**When** intenta registrarse

**Then** el sistema rechaza el registro

**And** no crea la cuenta

### Escenario 6: Rechazo de un correo con formato inválido

**Given** que el profesional informa un correo electrónico con formato inválido

**When** intenta registrarse

**Then** el sistema rechaza el registro

**And** no crea la cuenta

### Escenario 7: Rechazo de un correo que supera el máximo

**Given** que el profesional informa un correo electrónico de más de 254 caracteres

**When** intenta registrarse

**Then** el sistema rechaza el registro

**And** no crea la cuenta

### Escenario 8: Rechazo de un correo ya registrado

**Given** que ya existe una cuenta con el correo electrónico informado

**When** el profesional intenta registrarse con ese correo

**Then** el sistema rechaza el registro con el mensaje «El email ya está en uso»

**And** ofrece un enlace a la recuperación de contraseña

**And** no crea una segunda cuenta

### Escenario 9: Rechazo de una contraseña demasiado corta

**Given** que el profesional informa una contraseña de menos de 8 caracteres

**When** intenta registrarse

**Then** el sistema rechaza el registro con el mensaje «La contraseña es muy corta»

**And** no crea la cuenta

### Escenario 10: Rechazo de una contraseña sin mayúscula

**Given** que el profesional informa una contraseña de al menos 8 caracteres y con un número, pero sin ninguna letra mayúscula

**When** intenta registrarse

**Then** el sistema rechaza el registro

**And** no crea la cuenta

### Escenario 11: Rechazo de una contraseña sin número

**Given** que el profesional informa una contraseña de al menos 8 caracteres y con una letra mayúscula, pero sin ningún número

**When** intenta registrarse

**Then** el sistema rechaza el registro

**And** no crea la cuenta

### Escenario 12: Generación de una URL única ante nombres coincidentes

**Given** que ya existe en la plataforma una URL pública generada a partir del mismo nombre completo

**When** se completa el registro del nuevo profesional

**Then** el sistema agrega a la URL un sufijo numérico incremental que no esté utilizado

**And** la URL resultante es única en toda la plataforma

### Escenario 13: Normalizar nombre y correo antes del alta
**Given** que el profesional informa nombre y correo con espacios exteriores y mayúsculas en el correo
**When** envía un registro válido
**Then** el sistema recorta los espacios exteriores del nombre
**And** conserva sus caracteres Unicode y espacios internos
**And** guarda y compara el correo en minúsculas

### Escenario 14: Recuperar un onboarding incompleto
**Given** que Auth creó la cuenta pero el proceso de onboarding quedó incompleto
**When** el profesional vuelve a ingresar
**Then** el sistema reintenta idempotentemente los pasos faltantes
**And** no crea una segunda cuenta

### Escenario 15: Completar el alta aunque falle la bienvenida
**Given** que la cuenta, la sesión y el onboarding quedaron creados
**When** falla el envío del correo de bienvenida
**Then** el sistema conserva el alta exitosa
**And** registra el fallo para aplicar la política de reintentos

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-3 y decisiones transversales aplicables.

* El nombre se recorta al inicio y al final, conserva acentos y espacios internos, y debe contener entre 1 y 100 caracteres Unicode.
* El correo se recorta, se guarda en minúsculas y se compara sin distinguir mayúsculas. Su máximo es 254 caracteres.
* La contraseña debe contener entre 8 y 72 caracteres, al menos una mayúscula y un número.
* Mensajes aprobados:
  * Campo vacío: `Completa este campo.`
  * Nombre extenso: `El nombre no puede superar los 100 caracteres.`
  * Correo inválido: `Ingresa un correo electrónico válido.`
  * Correo extenso: `El correo no puede superar los 254 caracteres.`
  * Correo duplicado: `El email ya está en uso.` y enlace a recuperación.
  * Contraseña inválida: `La contraseña debe tener entre 8 y 72 caracteres, una mayúscula y un número.`
* No se exige confirmar el correo antes de utilizar la cuenta. El registro válido inicia sesión inmediatamente.
* Si se creó el usuario de Auth pero falló perfil, slug o configuración inicial, la cuenta queda en onboarding incompleto. El siguiente ingreso reintenta idempotentemente los pasos faltantes y nunca crea otra cuenta.
* Una falla del correo de bienvenida no revierte el alta. Se registra el fallo y se reintenta según la política de correos de la sección 7.

## Notas de QA

* Probar los límites de nombre con 100 y 101 caracteres, y los de correo con 254 y 255 caracteres.
* Cubrir contraseñas de 7 y 8 caracteres, con y sin mayúscula, y con y sin número.
* Usar cuentas y correos sintéticos en un entorno habilitado para pruebas; no crear datos ni enviar correos desde producción sin autorización.
* Verificar por separado los cinco efectos del flujo exitoso: cuenta, sesión, URL, redirección y correo.
* La implementación continúa `Sin verificar`; este refinamiento no confirma el comportamiento de la interfaz ni de Supabase.

## Inspección Shift-Left

**Resultado:** Aprobado

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-3.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| El profesional se registra con nombre, correo electrónico y contraseña | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 2.1 y 3.1 |
| Nombre obligatorio, no vacío y de hasta 100 caracteres | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.1 |
| Correo obligatorio, válido, de hasta 254 caracteres y no registrado previamente | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.1 |
| Contraseña obligatoria de al menos 8 caracteres, con una mayúscula y un número | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.1 |
| Mensajes «El email ya está en uso» y «La contraseña es muy corta» | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.1 |
| El correo duplicado ofrece un enlace a recuperación de contraseña | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.1 |
| El registro crea la cuenta y la sesión, genera la URL, redirige a la configuración y envía la bienvenida | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 3.1 y 7; `.context/architecture/prd.md` · Feature 1 y User Journeys |
| La URL se normaliza desde el nombre y resuelve colisiones con un sufijo numérico incremental | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.4 |
| Los correos de producto se envían mediante Resend | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · resumen del 03/03/2026 y correo del 28/02/2026 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-3 |

## Contradicciones detectadas

* Ninguna pendiente después de aplicar las decisiones de Producto para el release 1.1.

## Preguntas abiertas

* Ninguna pendiente de decisión funcional.
