# Story: Inicio y cierre de sesión del profesional

**ID:** CAQ-4
**Epic:** CAQ-2
**Implementación:** Parcial
**Modo de exploración:** Navegador automatizado
**Entorno observado:** Producción · 2026-09-07 / 2026-09-08
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

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| La pantalla `/login` carga un formulario con campos `Correo Electrónico`, `Contraseña`, enlace `¿Olvidaste tu contraseña?` y botón `Iniciar Sesión`. | `evidence/2026-09-07-formulario-login.png` | Coincide con la sección 3.2 de la especificación funcional (`.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md`). |
| Al ingresar credenciales válidas, autentica y redirige a `/dashboard`, mostrando el panel principal, enlaces de reservas y la opción `Salir` en la barra superior. | `evidence/2026-09-07-dashboard-post-login.png` | Coincide con la sección 3.2 y el flujo de navegación de la especificación funcional. |
| Al hacer clic en «Salir», la interfaz cambia la barra de navegación y redirige a `/login`, pero al navegar hacia atrás o ingresar directamente a `/dashboard`, la aplicación vuelve a renderizar pantallas privadas en lugar de redirigir a `/login`. | `evidence/2026-09-07-dashboard-post-login.png` | Contradice la especificación y la decisión de Producto, que exigen invalidación completa y redirección inmediata. |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| El inicio de sesión utiliza correo electrónico y contraseña | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.2 |
| Las credenciales incorrectas producen el mensaje «Email o contraseña incorrectos» sin revelar cuál falló | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.2 |
| La autenticación utiliza Supabase Auth, cookies `httpOnly` y middleware para proteger el dashboard | `.context/Confluence-corporativo/04-notas-tecnicas.md` · sección Auth; `.context/architecture/prd.md` · Requisitos No Funcionales |
| Carga de formulario de login accesible y elementos interactivos | **Observado** — Producción, 2026-09-07. Evidencia: `evidence/2026-09-07-formulario-login.png` |
| Una sesión válida permitió iniciar sesión y acceder a `/dashboard` | **Observado** — Producción, 2026-09-07. Evidencia: `evidence/2026-09-07-dashboard-post-login.png` |
| «Salir» cambió la navegación a las opciones de usuario no autenticado pero persistió renderizado en cliente tras navegación forzada | **Observado** — Producción, 2026-09-07. Evidencia: `evidence/2026-09-07-dashboard-post-login.png` |
| Sin sesión o después del logout, las rutas protegidas deben impedir el acceso y no exponer contenido de la cuenta | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-4 |
| Reglas aprobadas para el release 1.1 | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-4 |

## Contradicciones detectadas

* La especificación y la decisión de Producto exigen proteger `/dashboard/*`, limpiar los datos privados y redirigir a `/login`; producción permitió renderizar rutas después del logout. El requisito queda definido y la discrepancia pasa a investigación técnica.

## Preguntas abiertas

* ¿La exposición observada después del logout se debe a routing, caché, renderizado o autorización incompleta de la API?
