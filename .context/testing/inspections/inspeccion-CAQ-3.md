# Reporte de Inspección de Requisitos: Registro del profesional

**Historia:** CAQ-3  
**Fecha:** 2026-09-02  
**Estado de sincronización:** Sincronizado con Jira

## Resumen del requisito

La historia busca que un profesional cree una cuenta con nombre completo, correo electrónico y contraseña, inicie sesión automáticamente, obtenga una URL pública única, sea redirigido a la configuración inicial y reciba un correo de bienvenida.

La intención funcional y las validaciones principales están documentadas, pero el requisito combina cinco resultados observables y deja sin definir decisiones necesarias para probar la consistencia del alta ante fallos parciales, reintentos, normalización de datos y colisiones concurrentes. Además, omite la promesa de activación en menos de cinco minutos declarada en la especificación funcional histórica; el PRD solo conserva el journey de activación, sin ese umbral.

No se ejecutó el análisis contra el comportamiento observado: la historia indica `**Implementación:** Sin verificar` y su tabla de Fuentes no contiene filas `Observado`.

## 1. Defectos Encontrados

| ID | Tipo | Descripción del Defecto | Sugerencia de Corrección |
| :--- | :--- | :--- | :--- |
| CAQ-3-D01 | Ambigüedad | «Correo válido» no define la sintaxis aceptada, el tratamiento de dominios internacionalizados ni si se admiten alias con `+`. Distintas capas podrían aceptar conjuntos diferentes. | Definir una única regla de validación y aplicarla de forma consistente en cliente y servidor. **Hipótesis:** adoptar una validación compatible con el proveedor de autenticación hasta que Producto y Desarrollo la confirmen. |
| CAQ-3-D02 | Ambigüedad / límites | Los máximos de 100 y 254 caracteres no indican si se calculan antes o después de quitar espacios ni si cuentan caracteres Unicode, puntos de código o bytes. | Definir normalización y unidad de longitud antes de los escenarios de borde. **Hipótesis:** medir caracteres del valor normalizado. |
| CAQ-3-D03 | Ambigüedad | «Nombre completo» solo exige que no esté vacío; no define si un único nombre es válido, si un valor compuesto solo por espacios cuenta como vacío ni qué caracteres se admiten. | Acordar la regla mínima del nombre y ejemplos válidos/inválidos. No restringir alfabetos o signos sin decisión de negocio. |
| CAQ-3-D04 | Completitud / testabilidad | Solo se especifican mensajes para correo duplicado y contraseña corta. Los demás rechazos no tienen texto, ubicación, momento de aparición ni regla de conservación de datos. | Definir el resultado visible de cada validación y si los demás campos conservan su valor. Mientras no se acuerde el texto, exigir al menos un error asociado al campo, accesible y verificable. **Hipótesis.** |
| CAQ-3-D05 | Caso borde / integridad | No se define si el correo se compara sin distinguir mayúsculas y minúsculas ni si se eliminan espacios antes de comprobar duplicados. Esto puede permitir cuentas duplicadas o rechazos inconsistentes. | Acordar una forma canónica del correo para validación, unicidad y persistencia. **Hipótesis:** quitar espacios exteriores y comparar de forma no sensible a mayúsculas. |
| CAQ-3-D06 | Caso borde | No existe límite máximo de contraseña ni regla sobre espacios o caracteres Unicode. Una capa puede aceptarlos y otra rechazarlos o truncarlos. | Definir longitud máxima y conjunto admitido, alineados con el proveedor de autenticación, sin imponer una política no documentada. |
| CAQ-3-D07 | Integridad / estados | El alta reúne cuenta, sesión, URL, redirección y correo, pero no define el estado final si falla uno de esos pasos después de crear la cuenta. Un reintento podría encontrar el correo ocupado aunque la activación haya quedado incompleta. | Definir qué operaciones son atómicas, cuáles son reintentables y qué pantalla/acción de recuperación corresponde a cada fallo parcial. |
| CAQ-3-D08 | Resiliencia | No se define qué sucede si Resend rechaza, demora o no entrega el correo de bienvenida. Tampoco se distingue «solicitud aceptada» de «correo entregado». | Precisar si el correo bloquea el alta, política de reintento y evidencia verificable del envío. **Hipótesis:** desacoplar la bienvenida del alta y permitir reintento seguro. |
| CAQ-3-D09 | Caso borde / concurrencia | La regla de sufijo incremental no cubre dos altas simultáneas con el mismo nombre, ni qué ocurre si la inserción de la URL colisiona entre la consulta y el guardado. | Exigir unicidad en persistencia y definir un reintento acotado para regenerar el sufijo. **Hipótesis técnica:** restricción única más reintento. |
| CAQ-3-D10 | Completitud | La normalización de la URL no cubre signos, apóstrofes, guiones repetidos, caracteres no latinos ni un nombre que quede vacío después de normalizarse. | Incorporar ejemplos y resultado esperado para cada clase; dejar pendiente la estrategia de respaldo cuando no se pueda producir un slug. |
| CAQ-3-D11 | Resiliencia / idempotencia | No se define el resultado ante timeout, pérdida de conexión, doble clic o reenvío del formulario. El usuario podría recibir error pese a que la cuenta se creó, o provocar efectos duplicados. | Definir reintento e idempotencia del alta, bloqueo temporal del envío y recuperación tras resultado incierto. **Hipótesis.** |
| CAQ-3-D12 | Completitud / navegación | «Asistente de configuración inicial» no tiene ruta, estado inicial ni condición observable que permita confirmar que la redirección fue correcta. | Referenciar la historia o especificación del asistente y definir su primera pantalla. Hasta entonces, marcar este resultado como `Pendiente`. |
| CAQ-3-D13 | Omisión de alcance documentado | La historia no incorpora la promesa histórica de que el profesional pueda registrarse y tener su enlace funcionando en menos de cinco minutos. El PRD conserva el journey de activación, pero no ratifica ese umbral. | Solicitar ratificación del umbral; si continúa vigente, agregar un criterio medible del flujo completo y aclarar desde qué evento hasta cuál se miden los cinco minutos. |
| CAQ-3-D14 | Alcance / atomicidad | La propia evaluación INVEST indica que la historia no es pequeña, pero la historia conserva cinco efectos y dependencias sin una decisión de división o coordinación. Esto vuelve ambiguo qué entrega puede aceptarse por separado. | Dividir en historias trazables —alta y sesión, generación de URL, configuración inicial y bienvenida— o declarar explícitamente que CAQ-3 es un flujo integrador que solo se acepta cuando todas están completas. |
| CAQ-3-D15 | Trazabilidad / autoridad | Las reglas detalladas provienen principalmente de una especificación histórica en estado `BORRADOR — pendiente de revisión con desarrollo`. El PRD reconstruido respalda el flujo principal, pero no ratifica todos los mensajes y detalles. | Solicitar al Product Owner que confirme los detalles heredados antes de tratarlos como contrato definitivo; conservar su origen histórico en la trazabilidad. |

## 2. Versión Corregida de la Historia

### Story: Registro del profesional

**ID:** CAQ-3  
**Epic:** CAQ-2  
**Implementación:** Sin verificar  
**Estado:** Requiere cambios

#### Descripción

Como profesional, quiero registrarme con mi nombre completo, correo electrónico y contraseña, para crear mi cuenta, iniciar sesión y comenzar a configurar mi agenda.

#### Reglas confirmadas por la documentación

- Nombre completo: obligatorio, no vacío y de hasta 100 caracteres.
- Correo electrónico: obligatorio, con formato válido, de hasta 254 caracteres y no registrado previamente.
- Contraseña: obligatoria, de al menos 8 caracteres, con una mayúscula y un número.
- El alta exitosa crea la cuenta, inicia sesión automáticamente, genera una URL pública única, redirige a la configuración inicial y solicita el envío del correo de bienvenida.
- La URL se obtiene del nombre en minúsculas, sin acentos y con espacios convertidos en guiones; ante una colisión se agrega un sufijo numérico incremental libre.

#### Regla histórica pendiente de ratificación

- La especificación funcional histórica establece que la activación completa, desde el inicio del registro hasta contar con el enlace de reservas funcionando, debe poder realizarse en menos de cinco minutos. El PRD no conserva el umbral, por lo que Producto debe confirmar su vigencia.

#### Criterios de Aceptación candidatos

##### Escenario 1: Registro exitoso del profesional

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

##### Escenario 2: Rechazo de un campo obligatorio vacío

**Given** que el nombre completo, el correo electrónico o la contraseña está vacío  
**When** el profesional intenta enviar el formulario  
**Then** el sistema rechaza el registro  
**And** no crea la cuenta.

##### Escenario 3: Rechazo de nombre o correo que supera el máximo

**Given** que el nombre completo supera 100 caracteres o el correo electrónico supera 254 caracteres  
**When** el profesional intenta registrarse  
**Then** el sistema rechaza el registro  
**And** no crea la cuenta.

##### Escenario 4: Rechazo de un correo con formato inválido

**Given** que el profesional informa un correo electrónico con formato inválido  
**When** intenta registrarse  
**Then** el sistema rechaza el registro  
**And** no crea la cuenta.

##### Escenario 5: Rechazo de un correo ya registrado

**Given** que ya existe una cuenta con el correo informado  
**When** el profesional intenta registrarse con ese correo  
**Then** el sistema rechaza el registro con el mensaje «El email ya está en uso»  
**And** ofrece un enlace a la recuperación de contraseña  
**And** no crea una segunda cuenta.

##### Escenario 6: Rechazo por cada regla mínima de contraseña

**Given** que la contraseña tiene menos de 8 caracteres, o no contiene una mayúscula, o no contiene un número  
**When** el profesional intenta registrarse  
**Then** el sistema rechaza el registro  
**And** no crea la cuenta  
**And** si tiene menos de 8 caracteres muestra «La contraseña es muy corta».

##### Escenario 7: Generación de una URL única ante nombres coincidentes

**Given** que ya existe una URL pública generada a partir del mismo nombre completo  
**When** se completa el registro del nuevo profesional  
**Then** el sistema agrega un sufijo numérico incremental no utilizado  
**And** la URL resultante es única en toda la plataforma.

##### Escenario 8: Tiempo del flujo de activación, sujeto a ratificación

**Given** que el profesional cuenta con datos válidos y los servicios necesarios están disponibles  
**When** inicia el registro  
**Then** puede completar el alta y tener su enlace de reservas funcionando en menos de cinco minutos.

#### Decisiones pendientes antes de aprobar

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

> Los criterios anteriores son candidatos trazables a documentación existente; no quedan aprobados por esta inspección. Las decisiones pendientes no se completaron con supuestos.

## 3. Valoración de Calidad

- **Estado:** Requiere Cambios
- **Riesgo:** Alto

El flujo principal es comprensible, pero todavía no constituye un oráculo de prueba completo para fallos parciales, reintentos y datos límite. No se recomienda derivar el plan definitivo de pruebas hasta resolver las decisiones pendientes de mayor impacto.

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Descripción, criterios actuales, INVEST, preguntas y estado de implementación | `.context/PBI/epics/EPIC-CAQ-2-cuenta-y-activacion-del-profesional/stories/STORY-CAQ-3-registro-del-profesional/story.md` |
| CAQ-3 está indexada, sincronizada y pendiente de verificar contra la aplicación | `.context/PBI/epic-tree.md` · resumen y «Pendiente de verificar contra la aplicación» |
| Campos, límites, reglas de contraseña, resultados del alta, mensajes y generación de URL | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 3.1 y 3.4 |
| Promesa de activación con enlace funcionando en menos de cinco minutos | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 10 |
| Cuenta, sesión, URL única y journey de activación | `.context/architecture/prd.md` · Feature 1 y sección 4 |
| Los correos de producto migraron a Resend; autenticación permanece en Supabase | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · correos del 28/02/2026 y 03/03/2026 |
| Historia vigente consultada en Jira | Jira `CAQ-3` · consultada el 2026-09-02; actualización del issue: 2026-09-01 15:53:28 -03:00 |
| Versión corregida y trazabilidad del análisis | Jira `CAQ-3` · descripción actualizada el 2026-09-02; comentario de Shift-Left Testing `10190` |
| Validación uniforme, normalización, accesibilidad, idempotencia, atomicidad, restricción única y reintentos | **Hipótesis/recomendación** — no hay documento que defina estas soluciones |
| Comportamiento de la aplicación | `Pendiente` — el análisis 5 no aplica porque la historia está `Sin verificar` y no contiene fuentes `Observado` |

## Contradicciones detectadas

- No se detectó contradicción funcional entre CAQ-3 y el PRD para el flujo principal.
- Existe una omisión respecto de la especificación histórica: allí la activación con enlace operativo debe completarse en menos de cinco minutos, mientras CAQ-3 no la convierte en criterio verificable. El PRD conserva el journey, pero no el umbral. Se incorporó como criterio candidato sujeto a ratificación, no como hipótesis inventada.
- La historia afirma en INVEST que es testeable, pero sus propias preguntas abiertas impiden determinar el resultado esperado de normalización, errores y fallos parciales. Para la valoración se priorizó el contenido verificable de los criterios y las preguntas sobre la etiqueta INVEST.
- La especificación funcional histórica se declara borrador pendiente de revisión. El PRD más reciente confirma el núcleo del alta, pero no todos los detalles; por eso los detalles exclusivos del borrador requieren ratificación y no se trataron como evidencia actual observada.

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
