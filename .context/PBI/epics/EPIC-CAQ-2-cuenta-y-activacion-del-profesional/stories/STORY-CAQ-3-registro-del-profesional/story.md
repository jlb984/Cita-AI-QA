# Story: Registro del profesional

**ID:** CAQ-3
**Epic:** CAQ-2
**Implementación:** Sin verificar
**Estado de sincronización:** Sincronizado con Jira
**Última comprobación contra Jira:** 2026-09-02
**Estado:** Requiere cambios

## Descripción

Como profesional, quiero registrarme con mi nombre completo, correo electrónico y contraseña, para crear mi cuenta, iniciar sesión y comenzar a configurar mi agenda.

## Análisis INVEST

| Criterio | Cumple | Observación |
| :--- | :--- | :--- |
| Independiente | Parcial | El flujo depende de autenticación, generación de URL, configuración inicial y correo. Debe tratarse como flujo integrador o dividirse explícitamente. |
| Negociable | Sí | La solución técnica continúa abierta; las reglas sin acuerdo permanecen identificadas como decisiones pendientes. |
| Valiosa | Sí | Permite que el profesional cree su cuenta y comience la activación necesaria para recibir reservas. |
| Estimable | Parcial | El flujo principal está delimitado, pero los fallos parciales, reintentos y normalización todavía afectan la estimación. |
| Pequeña | No | Incluye cuenta, sesión, URL, redirección y correo. Producto debe decidir si permanece como flujo integrador o se divide. |
| Testeable | Parcial | Los criterios documentados pueden probarse, pero las decisiones pendientes impiden construir un oráculo completo. La implementación no fue verificada. |

## Reglas confirmadas por la documentación

- Nombre completo: obligatorio, no vacío y de hasta 100 caracteres.
- Correo electrónico: obligatorio, con formato válido, de hasta 254 caracteres y no registrado previamente.
- Contraseña: obligatoria, de al menos 8 caracteres, con una mayúscula y un número.
- El alta exitosa crea la cuenta, inicia sesión automáticamente, genera una URL pública única, redirige a la configuración inicial y solicita el envío del correo de bienvenida.
- La URL se obtiene del nombre en minúsculas, sin acentos y con espacios convertidos en guiones; ante una colisión se agrega un sufijo numérico incremental libre.

## Regla histórica pendiente de ratificación

- La especificación funcional histórica establece que la activación completa, desde el inicio del registro hasta contar con el enlace de reservas funcionando, debe poder realizarse en menos de cinco minutos. El PRD conserva el journey de activación, pero no el umbral; Producto debe confirmar su vigencia.

## Criterios de Aceptación candidatos

### Escenario 1: Registro exitoso del profesional

**Given** que el correo electrónico no está registrado
**And** el profesional informa un nombre completo no vacío de hasta 100 caracteres
**And** informa un correo válido de hasta 254 caracteres
**And** informa una contraseña de al menos 8 caracteres con una mayúscula y un número
**When** envía el formulario de registro
**Then** el sistema crea una única cuenta
**And** inicia la sesión automáticamente
**And** genera una URL pública única
**And** redirige al asistente de configuración inicial
**And** solicita el envío del correo de bienvenida.

### Escenario 2: Rechazo de un campo obligatorio vacío

**Given** que el nombre completo, el correo electrónico o la contraseña está vacío
**When** el profesional intenta enviar el formulario
**Then** el sistema rechaza el registro
**And** no crea la cuenta.

### Escenario 3: Rechazo de nombre o correo que supera el máximo

**Given** que el nombre completo supera 100 caracteres o el correo electrónico supera 254 caracteres
**When** el profesional intenta registrarse
**Then** el sistema rechaza el registro
**And** no crea la cuenta.

### Escenario 4: Rechazo de un correo con formato inválido

**Given** que el profesional informa un correo electrónico con formato inválido
**When** intenta registrarse
**Then** el sistema rechaza el registro
**And** no crea la cuenta.

### Escenario 5: Rechazo de un correo ya registrado

**Given** que ya existe una cuenta con el correo informado
**When** el profesional intenta registrarse con ese correo
**Then** el sistema rechaza el registro con el mensaje «El email ya está en uso»
**And** ofrece un enlace a la recuperación de contraseña
**And** no crea una segunda cuenta.

### Escenario 6: Rechazo por cada regla mínima de contraseña

**Given** que la contraseña tiene menos de 8 caracteres, no contiene una mayúscula o no contiene un número
**When** el profesional intenta registrarse
**Then** el sistema rechaza el registro
**And** no crea la cuenta
**And** si tiene menos de 8 caracteres muestra «La contraseña es muy corta».

### Escenario 7: Generación de una URL única ante nombres coincidentes

**Given** que ya existe una URL pública generada a partir del mismo nombre completo
**When** se completa el registro del nuevo profesional
**Then** el sistema agrega un sufijo numérico incremental no utilizado
**And** la URL resultante es única en toda la plataforma.

### Escenario 8: Tiempo del flujo de activación, sujeto a ratificación

**Given** que el profesional cuenta con datos válidos y los servicios necesarios están disponibles
**When** inicia el registro
**Then** puede completar el alta y tener su enlace de reservas funcionando en menos de cinco minutos.

## Decisiones pendientes antes de aprobar

- Normalización y unidad de conteo de nombre y correo.
- Sintaxis exacta admitida para correo.
- Máximo y caracteres admitidos para contraseña.
- Mensajes y presentación accesible de los errores no documentados.
- Resultado y recuperación ante fallos parciales, timeout o reenvío.
- Normalización de URL para signos, alfabetos no latinos y resultados vacíos.
- Comportamiento ante colisiones concurrentes de URL.
- Ruta y primera pantalla del asistente de configuración.
- Inicio y fin exactos de la medición de cinco minutos.
- Confirmación del Product Owner sobre las reglas heredadas del borrador histórico.

> Estos criterios son candidatos trazables a documentación existente; no quedan aprobados por la sincronización. Las decisiones pendientes no se completaron con supuestos.

## Notas de QA

- Probar los límites de nombre con 100 y 101 caracteres, y los de correo con 254 y 255 caracteres una vez definida la unidad de conteo.
- Cubrir contraseñas de 7 y 8 caracteres, con y sin mayúscula, y con y sin número.
- Usar cuentas y correos sintéticos únicamente en un entorno habilitado para pruebas; no crear datos ni enviar correos desde producción sin autorización.
- Verificar por separado cuenta, sesión, URL, redirección y solicitud de envío del correo.
- Cubrir colisiones de URL y reenvíos cuando se definan sus resultados esperados.
- La implementación continúa `Sin verificar`; este documento no confirma el comportamiento de la interfaz, Supabase ni Resend.

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
| Umbral histórico de activación en menos de cinco minutos | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 10; pendiente de ratificación porque el PRD no conserva el valor |
| Defectos, criterios candidatos y decisiones pendientes | `.context/testing/inspections/inspeccion-CAQ-3.md` · inspección del 2026-09-02 |
| Versión vigente cotejada | `CAQ-3` · Jira, comprobado el 2026-09-02; ticket actualizado el 2026-09-02 18:09:26 -03:00 |

## Contradicciones detectadas

- La especificación exige una contraseña de al menos 8 caracteres, con una mayúscula y un número; `.context/PBI/epic-tree.md` y `.context/architecture/prd.md` indican que no existe evidencia actual de que la interfaz y Supabase apliquen exactamente esas reglas. Se conserva la especificación como comportamiento esperado y la implementación permanece `Sin verificar`.
- La especificación histórica fija menos de cinco minutos para la activación; el PRD conserva el journey, pero no ese umbral. El criterio queda sujeto a ratificación de Producto.
- Antes del cotejo, Jira contenía la versión corregida por la inspección del 2026-09-02 y este archivo conservaba la versión refinada previa. Se tomó la versión de Jira porque su origen está trazado al reporte local de inspección y no a un cambio externo desconocido; se preservaron las Fuentes, el estado de implementación y las preguntas locales.

## Preguntas abiertas

- ¿Cómo se define exactamente un correo válido y qué normalización se aplica antes de validar, comparar y guardar?
- ¿Los límites de nombre y correo se miden antes o después de normalizar, y en qué unidad?
- ¿Un único nombre es válido? ¿Un nombre compuesto solo por espacios se considera vacío? ¿Qué caracteres deben admitirse?
- ¿Cuál es la longitud máxima de contraseña y cómo se tratan espacios y caracteres Unicode?
- ¿Qué mensaje, ubicación y comportamiento de foco corresponde a cada validación sin texto acordado?
- ¿Qué estado queda y cómo continúa el usuario si se crea la cuenta, pero falla la sesión, la URL, la redirección o el correo?
- ¿El correo de bienvenida debe entregarse para considerar exitoso el registro o basta con aceptar su envío? ¿Cómo se reintenta?
- ¿Cómo se genera la URL con signos, apóstrofes, guiones repetidos, caracteres no latinos o un resultado normalizado vacío?
- ¿Cómo se resuelven colisiones simultáneas de URL y cuántos reintentos se permiten?
- ¿Qué respuesta recibe el usuario ante timeout, pérdida de conexión, doble envío o resultado incierto?
- ¿Cuál es la ruta y la primera pantalla esperada del asistente de configuración inicial?
- ¿Desde qué evento hasta cuál se mide la promesa de activación en menos de cinco minutos y bajo qué condiciones?
- ¿Producto ratifica los mensajes y detalles que solo figuran en la especificación histórica en estado borrador?
- ¿CAQ-3 se dividirá en historias más pequeñas o permanecerá como flujo integrador con dependencias explícitas?
- ¿El registro requiere confirmar el correo electrónico antes de permitir usar la cuenta, o la sesión automática queda habilitada inmediatamente?
