# Story: Inicio y cierre de sesión del profesional

**ID:** CAQ-4
**Epic:** CAQ-2
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Refinamiento:** Refinado
**Inspección QA:** Bloqueante

## Descripción

Como profesional, quiero iniciar sesión con mi correo electrónico y contraseña y cerrar la sesión cuando termine, para acceder a mi panel y proteger la información de mi cuenta.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Sí | Puede validarse con una cuenta profesional existente, sin depender de las funciones de agenda o reservas. |
| Negociable | Sí | El objetivo de acceso seguro está definido; la navegación y la presentación de errores siguen siendo negociables. |
| Valiosa | Sí | Permite al profesional acceder a su panel y finalizar el acceso cuando deja de utilizarlo. |
| Estimable | Sí | Las decisiones vigentes de Producto cierran los valores y resultados necesarios para estimar la Story. |
| Pequeña | No | Reúne inicio de sesión, cierre de sesión y protección de rutas. Si la incertidumbre impide completarla, dividir en acceso autenticado y revocación de acceso. |
| Testeable | Sí | Los criterios incorporan resultados observables y valores aprobados para el release 1.1. |

## Criterios de Aceptación (Gherkin)

### Escenario 1: Inicio de sesión exitoso

**Given** que existe una cuenta profesional y el usuario no tiene una sesión activa

**When** informa el correo electrónico y la contraseña correctos

**Then** el sistema inicia la sesión

**And** permite acceder al dashboard del profesional

### Escenario 2: Intento con un correo incorrecto

**Given** que el profesional no tiene una sesión activa

**When** intenta iniciar sesión con un correo electrónico incorrecto

**Then** el sistema rechaza el acceso con el mensaje «Email o contraseña incorrectos»

**And** no indica cuál de las credenciales falló

### Escenario 3: Intento con una contraseña incorrecta

**Given** que el profesional no tiene una sesión activa

**When** intenta iniciar sesión con una contraseña incorrecta

**Then** el sistema rechaza el acceso con el mensaje «Email o contraseña incorrectos»

**And** no indica cuál de las credenciales falló

### Escenario 4: Intento sin correo electrónico
**Given** que el profesional deja vacío el correo electrónico
**When** intenta iniciar sesión
**Then** el sistema no inicia la sesión
**And** muestra «Ingresa tu correo y contraseña.»
**And** no permite acceder al dashboard

### Escenario 5: Intento sin contraseña
**Given** que el profesional deja vacía la contraseña
**When** intenta iniciar sesión
**Then** el sistema no inicia la sesión
**And** muestra «Ingresa tu correo y contraseña.»
**And** no permite acceder al dashboard

### Escenario 6: Acceso al dashboard con una sesión válida

**Given** que el profesional tiene una sesión válida

**When** abre una ruta protegida del dashboard

**Then** el sistema muestra únicamente el contenido correspondiente a su cuenta

### Escenario 7: Cierre de una sesión activa

**Given** que el profesional tiene una sesión activa

**When** selecciona la opción «Salir»

**Then** el sistema finaliza la sesión

**And** la navegación deja de presentar las opciones exclusivas de una sesión activa

### Escenario 8: Acceso directo al dashboard sin sesión

**Given** que el profesional no tiene una sesión activa

**When** intenta abrir directamente una ruta protegida del dashboard

**Then** el sistema impide el acceso

**And** no expone contenido correspondiente a la cuenta profesional

### Escenario 9: Reingreso a una ruta protegida después del logout

**Given** que el profesional cerró su sesión

**When** vuelve a abrir una ruta protegida del dashboard

**Then** el sistema impide el acceso

**And** no expone contenido correspondiente a la sesión finalizada

### Escenario 10: Cerrar sesión en todas las pestañas del navegador
**Given** que el profesional tiene una sesión activa en dos pestañas del mismo navegador
**When** selecciona «Salir» en una de ellas
**Then** el sistema invalida la sesión actual en ambas pestañas
**And** elimina los datos privados de memoria y caché
**And** redirige a /login

### Escenario 11: Mantener las sesiones de otros dispositivos
**Given** que el profesional tiene sesiones válidas en dos dispositivos
**When** cierra la sesión en uno de ellos
**Then** el sistema invalida únicamente la sesión del dispositivo actual
**And** mantiene activa la sesión del otro dispositivo

### Escenario 12: Aplicar el bloqueo temporal de acceso
**Given** que una combinación de cuenta e IP acumula cinco intentos fallidos dentro de quince minutos
**When** se intenta iniciar sesión nuevamente durante los quince minutos siguientes
**Then** el sistema rechaza temporalmente el intento
**And** mantiene una respuesta genérica que no revela si la cuenta existe

## Decisiones de Producto incorporadas

Fuente vigente: `.context/PBI/decisiones-po-proximo-release.md` · CAQ-4 y decisiones transversales aplicables.

* El logout invalida la sesión actual, limpia de memoria y caché todos los datos privados y redirige inmediatamente a `/login`.
* La invalidación se propaga a todas las pestañas del mismo navegador. Las sesiones de otros dispositivos permanecen activas; cerrar todas las sesiones será una acción separada futura.
* Cualquier ruta `/dashboard/*` sin sesión válida redirige a `/login` y no entrega datos privados desde la API.
* Campo faltante: `Ingresa tu correo y contraseña.` Credenciales incorrectas: `Email o contraseña incorrectos.`
* Después de 5 intentos fallidos dentro de 15 minutos, se bloquean nuevos intentos durante 15 minutos por combinación de cuenta e IP. La respuesta continúa siendo genérica para no revelar cuentas existentes.
* El renderizado observado después del logout se considera defecto hasta demostrar que no existe exposición. Desarrollo debe investigar routing, caché, renderizado y autorización de API; la causa no puede decidirse desde Producto.

## Notas de QA

* Ejecutar los intentos con correo incorrecto y contraseña incorrecta por separado para comprobar que ambos devuelven el mismo mensaje genérico.
* Verificar el acceso directo a `/dashboard`, `/dashboard/availability` y `/dashboard/clients` con sesión válida, sin sesión y después del logout.
* Comprobar el cierre de sesión en la pestaña actual y el efecto sobre otras pestañas abiertas, sin asumir el resultado hasta que producto responda la pregunta abierta.
* No usar credenciales ni información personal reales; el único entorno documentado es producción y no está autorizado modificar datos durante este refinamiento.
* La implementación continúa `Sin verificar`; los datos observados documentan una brecha, pero este refinamiento no comprueba que siga vigente.

## Inspección Shift-Left

**Resultado:** Bloqueante

**Reporte:** `.context/testing/inspections/inspeccion-CAQ-4.md`

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| El inicio de sesión utiliza correo electrónico y contraseña | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.2 |
| Las credenciales incorrectas producen el mensaje «Email o contraseña incorrectos» sin revelar cuál falló | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.2 |
| La autenticación utiliza Supabase Auth, cookies `httpOnly` y middleware para proteger el dashboard | `.context/Confluence-corporativo/04-notas-tecnicas.md` · sección Auth; `.context/architecture/prd.md` · Requisitos No Funcionales |
| Una sesión válida permitió iniciar sesión y acceder a `/dashboard` | **Observado** — producción, 30/08/2026. Evidencia: `.context/architecture/prd.md` · Feature 1 y Fuentes |
| «Salir» cambió la navegación a las opciones de usuario no autenticado | **Observado** — producción, 30/08/2026. Evidencia: `.context/architecture/prd.md` · Seguridad observada |
| Después del logout, `/dashboard`, `/dashboard/availability` y `/dashboard/clients` continuaron renderizando sus pantallas | **Observado** — producción, 30/08/2026. Evidencia: `.context/architecture/prd.md` · Seguridad observada y Fuentes |
| Sin sesión o después del logout, las rutas protegidas deben impedir el acceso y no exponer contenido de la cuenta | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-4 |
| Reglas aprobadas para el release 1.1 | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-4 |

## Contradicciones detectadas

* La especificación y la decisión de Producto exigen proteger /dashboard/*, limpiar los datos privados y redirigir a /login; producción permitió renderizar rutas después del logout el 30/08/2026. El requisito queda definido y la discrepancia pasa a investigación técnica.

## Preguntas abiertas

* ¿La exposición observada después del logout se debe a routing, caché, renderizado o autorización incompleta de la API?
