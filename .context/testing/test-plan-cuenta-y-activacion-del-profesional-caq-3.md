# Plan de Pruebas: Registro del profesional

**Epic:** CAQ-2 — Cuenta y activación del profesional
**Historia:** CAQ-3 — Registro del profesional
**Fecha:** 2026-09-02
**Estado del plan:** Diseñado; ejecución bloqueada para pruebas mutantes

## Alcance

Este plan cubre exclusivamente `CAQ-3`: validación de nombre, correo y contraseña; creación de una única cuenta; inicio automático de sesión; generación de URL pública única; redirección a la configuración inicial; solicitud del correo de bienvenida; y los riesgos de consistencia asociados al flujo.

Las historias hermanas `CAQ-4`, `CAQ-5` y `CAQ-6` se consultaron para identificar dependencias y evitar duplicar alcance. La protección posterior al logout, la recuperación de contraseña y la exposición/copia posterior de la URL se validarán en sus respectivos planes, salvo la integración mínima necesaria para comprobar los efectos de `CAQ-3`.

No se ejecutarán altas ni envíos en Producción. El único entorno confirmado contiene usuarios reales, no dispone de reset documentado y cualquier registro puede persistir y enviar correos externos.

## 1. Matriz de Riesgos del Producto

Escala utilizada: probabilidad × impacto. `1–4 Bajo`, `5–9 Medio`, `10–16 Alto`, `17–25 Crítico`.

| ID | Riesgo | Probabilidad (1-5) | Impacto (1-5) | Nivel | Mitigación |
| :--- | :--- | :---: | :---: | :--- | :--- |
| R1 | Ejecutar el alta en el único entorno disponible puede crear PII, cuentas y correos persistentes sin aislamiento ni rollback de datos. | 5 | 5 | 25 (Crítico) | Bloquear pruebas mutantes en Producción; habilitar un entorno aislado con datos sintéticos, captura de correo, seed y teardown por `run_id`. |
| R2 | Un fallo parcial puede dejar creada la cuenta sin sesión, URL, redirección o bienvenida; el reintento puede quedar bloqueado por correo duplicado. | 4 | 5 | 20 (Crítico) | Pruebas unitarias del orquestador e integración con fallos inyectados en cada dependencia; acordar atomicidad, compensación y reintentos antes del veredicto. |
| R3 | Reenvíos, timeouts o altas concurrentes pueden crear efectos duplicados o URLs en conflicto porque no se definieron idempotencia ni resolución concurrente de colisiones. | 4 | 4 | 16 (Alto) | Pruebas de concurrencia e idempotencia en API/base; restricción única y reintento son **hipótesis técnicas** pendientes de diseño. |
| R4 | Las reglas ambiguas de normalización, longitud y caracteres pueden producir discrepancias entre UI, API, Supabase Auth y base de datos, o permitir duplicados por variantes del correo. | 4 | 4 | 16 (Alto) | Tabla de decisión y pruebas de frontera/particiones en todos los niveles; Producto y Desarrollo deben cerrar el oráculo antes de aprobar resultados hoy indefinidos. |
| R5 | El correo de bienvenida puede demorarse, fallar o ir a spam por la integración con Resend y su dominio no verificado; la historia no define si eso invalida el alta. | 4 | 3 | 12 (Alto) | Pruebas de contrato con Resend, mail sink en ambiente aislado, simulación de errores y métricas de aceptación/entrega; definir si el envío es bloqueante y su política de reintento. |

## 2. Niveles de Prueba (Pyramid)

- **Unitarias:** cubrir validadores de campos y fronteras; normalización de nombre/correo; generación determinista de slug; selección del siguiente sufijo; clasificación de errores; control de doble envío; y estados del orquestador ante éxito o fallo de cada dependencia. Las reglas aún no acordadas se parametrizan o se dejan pendientes; no se codifican como decisiones implícitas. El repositorio de QA no contiene el código de producto ni permite confirmar qué pruebas unitarias existen hoy.
- **Integración:** validar los contratos entre la API de registro, Supabase Auth, PostgreSQL/perfil profesional, creación del slug/su unicidad, sesión/cookie, Resend y destino de configuración inicial. Inyectar timeout, `4xx`, `5xx`, rechazo de correo y colisión concurrente. Verificar persistencia final, ausencia de duplicados, trazabilidad y comportamiento del reintento. Requiere entorno no productivo y controles de fallo que hoy no están documentados.
- **E2E:** recorrer un alta válida y verificar por separado cuenta, sesión, URL, redirección y solicitud del correo; cubrir campos vacíos, fronteras 100/101 y 254/255, correo inválido/duplicado, contraseña de 7/8 caracteres y ausencia de mayúscula/número. Agregar doble clic, recarga, timeout recuperable y dos altas simultáneas con el mismo nombre cuando se definan sus resultados. No ejecutar estos flujos en Producción.
- **Pruebas de componente/API:** probar las validaciones del servidor sin depender de la UI y confirmar que no sea posible evadirlas enviando solicitudes directas. Verificar contratos y códigos de respuesta una vez documentados; actualmente son `Pendiente`.
- **Pruebas exploratorias:** explorar combinaciones de espacios, mayúsculas, alias con `+`, Unicode, apóstrofes, guiones y nombres que produzcan slugs vacíos. Estos recorridos sirven para descubrir comportamiento, no para aprobar reglas que Producto aún no definió.

## Cobertura priorizada y trazabilidad

| Prioridad | Área / criterio | Cobertura prevista | Estado del oráculo |
| :--- | :--- | :--- | :--- |
| P0 | Alta exitosa y cinco efectos | Unitarias, integración y E2E | Parcial: falta definir fallos parciales y si el correo bloquea el alta |
| P0 | No duplicación por correo, reenvío o concurrencia | Integración/API y concurrencia | Pendiente: normalización e idempotencia no acordadas |
| P0 | URL única y colisiones | Unitarias e integración concurrente | Parcial: sufijo secuencial documentado; carrera simultánea pendiente |
| P1 | Obligatorios y límites documentados | Unitarias, API y E2E | Parcial: falta unidad de conteo y normalización |
| P1 | Reglas mínimas de contraseña | Unitarias, API y E2E | Parcial: falta máximo y tratamiento de Unicode/espacios |
| P1 | Correo duplicado y recuperación ofrecida | Integración y E2E | Documentado; requiere datos aislados y enlace a `CAQ-5` |
| P1 | Redirección a configuración inicial | Integración y E2E | Pendiente: ruta y primera pantalla no definidas |
| P2 | Activación en menos de cinco minutos | E2E y medición temporal | Pendiente de ratificación: valor histórico ausente del PRD actual |

## 3. Pruebas No Funcionales

- **Seguridad:** comprobar validación del lado servidor, manejo seguro de contraseña, flags y alcance de la sesión, ausencia de secretos/PII en URL, logs y errores, resistencia a automatización abusiva y enumeración de cuentas. La existencia de rate limiting, CAPTCHA y política frente a abuso no está documentada; cualquier implementación propuesta es **hipótesis**. El mensaje de correo duplicado está explícitamente documentado, por lo que un cambio a respuesta no enumerativa requiere decisión de Producto.
- **Performance:** medir tiempo de respuesta del envío, creación de cuenta, generación de URL y redirección bajo carga nominal y alta concurrente. No hay SLA acordado. El umbral de activación menor a cinco minutos es histórico y requiere ratificación, además de definir inicio, fin y condiciones de medición.
- **Concurrencia:** lanzar altas simultáneas con mismo nombre y con variantes equivalentes del mismo correo; verificar una sola cuenta por identidad acordada, URLs únicas y estado consistente. Volumen y concurrencia objetivo están pendientes.
- **Accesibilidad:** validar asociación de labels, anuncios de error, foco tras el rechazo, navegación por teclado, contraste y estado de carga. No existe estándar ni nivel de conformidad acordado; usar WCAG como criterio contractual sería una **hipótesis** hasta aprobación.
- **Compatibilidad:** comprobar el flujo responsive en la matriz de navegadores, sistemas, dispositivos y resoluciones que defina Producto. La matriz todavía no existe, por lo que no puede cerrarse la cobertura.
- **Resiliencia y observabilidad:** verificar timeout, reintento, indisponibilidad parcial, correlación de eventos y ausencia de efectos huérfanos. No hay monitoreo, alertas ni dashboard de errores documentados.

## 4. Necesidades de Entorno y Datos

| Necesidad | Entorno | ¿Disponible hoy? | Referencia |
| :--- | :--- | :--- | :--- |
| Entorno aislado para crear y eliminar cuentas sin afectar usuarios reales | No existe un entorno no productivo confirmado | No | `environments.md` · Mapa de Entornos; `test-data-strategy.md` · Fuentes de Datos |
| Seed y teardown idempotentes con limpieza limitada por `run_id` | No existe un entorno no productivo confirmado | No | `test-data-strategy.md` · Generación de Datos Sintéticos y Limpieza y Reset |
| Buzones sintéticos controlados y mail sink que impida envíos externos | Producción usa Resend/Supabase; entorno aislado pendiente | No | `environments.md` · Mapa de Entornos; `test-data-strategy.md` · Preguntas abiertas |
| Profesional nuevo con nombre/correo únicos y contraseña referenciada desde almacenamiento seguro | Entorno aislado pendiente | No | `test-data-strategy.md` · Gestión de Usuarios de Prueba y Credenciales |
| Cuenta preexistente para validar correo duplicado sin alterar datos reales | Producción | Parcial; no autorizada para prueba mutante | `test-data-strategy.md` · Gestión de Usuarios de Prueba y Restricción inmediata |
| Dos altas sintéticas con el mismo nombre para colisión de slug | Entorno aislado pendiente | No | `story.md` · Escenario 7; `test-data-strategy.md` · restricción de pruebas mutantes |
| Variantes del mismo correo en mayúsculas, espacios y alias `+` | Entorno aislado pendiente | No | `story.md` · Preguntas abiertas; dato propuesto como **hipótesis de diseño de prueba** |
| Contraseñas sintéticas para fronteras 7/8, sin mayúscula y sin número | Generables localmente; alta requiere entorno aislado | Parcial | `story.md` · Notas de QA |
| Inyección controlada de timeout y errores de Supabase/Resend | Entorno aislado o dobles de servicio pendientes | No | `inspeccion-CAQ-3.md` · CAQ-3-D07, D08 y D11 |
| Ejecución concurrente y consulta segura de persistencia para comprobar unicidad | Entorno aislado con acceso QA pendiente | No | `inspeccion-CAQ-3.md` · CAQ-3-D05 y D09; `environments.md` · Detalles de Acceso |
| Matriz aprobada de navegadores, dispositivos y resoluciones | Todos | No | `environments.md` · Tipo de Aplicación y Preguntas abiertas |
| Logs, métricas y correlación de eventos sin exponer PII | Producción tiene acceso QA no confirmado; entorno aislado pendiente | No | `environments.md` · Ausencias relevantes y Observabilidad |

### Condiciones de entrada

- Criterios candidatos aprobados o decisiones pendientes clasificadas como fuera de alcance explícito.
- Entorno aislado desplegado y nombrado oficialmente.
- Seed, fixtures, buzones sintéticos, mail sink y teardown validados.
- Accesos referenciados desde almacenamiento seguro; ninguna credencial en el plan.
- Contratos de API/códigos de respuesta y mecanismo de observación de Supabase/Resend disponibles.
- Autorización para ejecutar pruebas de concurrencia, resiliencia y seguridad.

### Condiciones de salida

- Todos los escenarios P0 ejecutados y aprobados, sin cuentas, perfiles, URLs ni correos duplicados o huérfanos.
- Escenarios P1 aprobados o defectos aceptados formalmente con riesgo residual.
- Teardown verificado por identificador de ejecución y sin afectar datos ajenos.
- Evidencia de los cinco efectos del alta y de los fallos parciales definidos.
- Sin defectos abiertos críticos o altos en seguridad, integridad, idempotencia o unicidad.
- Cobertura, resultados, defectos y riesgos residuales trazados a `CAQ-3`.

## 5. Herramientas sugeridas

- **Runner unitario y de integración del producto:** usar el framework ya adoptado por el repositorio de la aplicación; no puede determinarse desde este repositorio de QA.
- **Playwright:** automatización E2E, accesibilidad básica del flujo, concurrencia coordinada desde varias sesiones y captura de evidencia. Ya fue utilizado para observación de solo lectura.
- **Cliente HTTP/Postman o equivalente:** pruebas directas de API y contratos cuando los endpoints estén documentados.
- **k6:** carga y concurrencia controlada en un entorno autorizado. Su adopción es una **hipótesis de herramienta**.
- **OWASP ZAP:** análisis dinámico de seguridad en un entorno autorizado, con alcance y límites acordados. Su adopción es una **hipótesis de herramienta**.
- **Mail sink compatible con Resend/Supabase:** captura de mensajes sin entrega externa. El producto no documenta uno actualmente.
- **Mocks/fault injection:** simulación determinista de timeout, `4xx`, `5xx` y fallos parciales de Supabase y Resend.

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Alcance, criterios candidatos, límites y estado `Sin verificar` de CAQ-3 | `.context/PBI/epics/EPIC-CAQ-2-cuenta-y-activacion-del-profesional/stories/STORY-CAQ-3-registro-del-profesional/story.md` |
| Dependencias con sesión, recuperación y exposición de URL | `epic.md`; `STORY-CAQ-4-inicio-y-cierre-de-sesion/story.md`; `STORY-CAQ-5-recuperacion-de-contrasena/story.md`; `STORY-CAQ-6-acceso-a-url-publica/story.md` |
| Fallos parciales, normalización, concurrencia, idempotencia y decisiones pendientes | `.context/testing/inspections/inspeccion-CAQ-3.md` · defectos D01–D15 y Preguntas abiertas |
| Producción única, servicios, ausencia de CI/monitoreo y restricciones de acceso | `.context/infrastructure/environments.md` · secciones 2–5 |
| Ausencia de datos mutables seguros, seed, teardown y mail sink | `.context/infrastructure/test-data-strategy.md` · secciones 1–5 |
| Riesgo R1 y bloqueo de pruebas mutantes | `environments.md` · Riesgos del Mapa de Entornos; `test-data-strategy.md` · Restricción inmediata |
| Riesgos R2–R5 y valores de probabilidad/impacto | Riesgos trazados a `inspeccion-CAQ-3.md`, `environments.md` y `test-data-strategy.md`; las puntuaciones son **valoración del Test Manager** |
| Normalización uniforme, idempotencia, restricción única, compensación y reintentos | **Hipótesis/recomendación técnica** — no existe diseño acordado |
| Rate limiting, CAPTCHA, WCAG, k6, OWASP ZAP y mail sink | **Hipótesis/recomendación** — no existe adopción ni requisito acordado |

## Contradicciones detectadas

- La especificación funcional histórica declara UAT con datos ficticios; las notas técnicas describen una copia parcialmente anonimizada de producción y la nota más reciente indica que UAT dejó de usarse. Se toma como vigente que solo Producción está confirmada y se bloquean allí las pruebas mutantes.
- La especificación histórica fija una activación con enlace operativo en menos de cinco minutos; el PRD conserva el journey, pero no el umbral. La prueba se diseña, pero su aprobación queda pendiente de ratificación y de una definición de medición.
- La especificación exige reglas concretas de contraseña, mientras el PRD y el índice indican que no existe evidencia actual de que la interfaz y Supabase las apliquen. Se conserva la regla como expectativa documentada y la implementación permanece sin verificar.
- La documentación dice que la URL se genera durante el registro; soporte y la observación de producción indican que no se encontró expuesta en la experiencia autenticada. Para `CAQ-3` se valida la generación; la localización y copia pertenecen a `CAQ-6`.

## Preguntas abiertas

- ¿Cuál será el nombre y la URL del entorno aislado autorizado para estas pruebas?
- ¿Quién proveerá y mantendrá seed, factories, mail sink y teardown, y cómo se comprobará que la limpieza solo afecta al `run_id` de la ejecución?
- ¿Cómo se consultarán de forma segura Supabase Auth, perfiles, slugs, sesión y eventos de Resend para verificar cada efecto?
- ¿Qué operaciones del alta deben ser atómicas y cómo se recupera cada fallo parcial?
- ¿Qué garantía de idempotencia existe ante doble envío, timeout o reintento?
- ¿Cómo se normaliza el correo y en qué unidad se miden los límites de nombre y correo?
- ¿Qué sintaxis de correo, caracteres de nombre y longitud máxima de contraseña se admiten?
- ¿Cómo se resuelven colisiones concurrentes de URL y cuál es el límite de reintentos?
- ¿Qué ruta y primera pantalla definen una redirección correcta al asistente inicial?
- ¿La aceptación del envío o la entrega del correo de bienvenida forma parte del éxito? ¿Cuál es la política de reintento?
- ¿Producto ratifica el umbral de activación menor a cinco minutos? ¿Cómo se miden su inicio, fin y condiciones?
- ¿Qué concurrencia y tiempos de respuesta constituyen los objetivos de performance?
- ¿Qué matriz de navegadores, dispositivos, resoluciones y nivel de accesibilidad debe cubrirse?
- ¿Qué controles contra abuso y automatización del registro son obligatorios?
- ¿CAQ-3 seguirá siendo un flujo integrador o se dividirá en historias con aceptación independiente?

## Secciones incompletas o bloqueadas

- **Ejecución E2E, integración, concurrencia, seguridad dinámica y correo:** bloqueadas porque no existe un entorno aislado ni datos mutables autorizados.
- **Oráculos de normalización, fallos parciales, redirección e idempotencia:** incompletos por decisiones abiertas de `CAQ-3`.
- **Performance:** incompleta por falta de SLA, carga objetivo y ratificación del umbral histórico.
- **Compatibilidad y accesibilidad:** incompletas porque no hay matriz ni estándar acordados.
- **Cobertura unitaria existente:** desconocida porque este repositorio no contiene el código ni los tests del producto.
- **Observabilidad y teardown:** bloqueados por falta de acceso QA, seed, reset y herramientas documentadas.

## Próximo paso

Resolver primero los bloqueantes de entorno, datos y criterios. Una vez aprobado el plan y disponible un entorno aislado, iniciar la ejecución con `.prompts/6-Testing Exploratorio/smoke-test.md`.
