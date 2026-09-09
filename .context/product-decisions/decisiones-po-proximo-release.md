# Decisiones de Producto para el próximo release de Cita AI

**Responsable:** Product Owner de la demostración

**Fecha:** 03/09/2026

**Estado:** Aprobado para refinamiento

**Release objetivo:** Estabilización 1.1

**Ubicación funcional:** Backlog y decisiones vigentes de Producto

## 1. Propósito y autoridad

Este documento responde las preguntas abiertas detectadas durante el refinamiento y la inspección Shift-Left. Sus decisiones son la fuente funcional vigente para reprocesar las Epics y Stories que se incorporen al proyecto Jira `CAQ`.

Cuando una decisión contradiga una especificación histórica, prevalece este documento por ser posterior. El comportamiento observado continúa siendo evidencia de la implementación, no una regla de negocio. Una diferencia entre este documento y la aplicación debe tratarse como defecto o trabajo pendiente.

El respaldo `.context/Confluence-corporativo/` permanece inmutable. Estas son decisiones nuevas para el siguiente release, no modificaciones retroactivas de lo que se acordó al inicio del proyecto.

## 2. Alcance y decisiones transversales

### 2.1 Objetivo del release

El release 1.1 será de estabilización y completitud del alcance existente. Incluye cuenta, agenda, página pública, reservas, cancelaciones, correos, recordatorios, clientes y límite freemium.

Quedan fuera de alcance pagos, sincronización con calendarios, múltiples profesionales por organización, múltiples servicios, precios, formularios personalizados, aplicaciones nativas y una contratación real del Plan Pro.

El proyecto Jira canónico de esta ejecución es `CAQ`, en
`https://jlb984.atlassian.net/`. Todas las referencias de este documento usan las claves
reales obtenidas al deduplicar y adaptar el backlog importado de `BJHB` el 06/09/2026. El detalle de equivalencias
se conserva en `.context/PBI/reconciliacion-backlog-caq.md`.

### 2.2 Zona horaria y fechas

* Cada profesional tendrá una zona horaria IANA obligatoria. Al registrarse se propone la zona del navegador y el profesional puede confirmarla o cambiarla.
* Los instantes se almacenan en UTC.
* Dashboard, correos y página pública presentan fecha y hora en la zona del profesional e incluyen su identificador o abreviatura visible.
* En este release no se convierte automáticamente la hora a la zona del cliente. La página pública debe aclarar que los horarios corresponden a la zona del profesional.
* La API conserva timestamps ISO 8601 en UTC.

### 2.3 Ventanas de reserva y cancelación

* Una reserva pública puede confirmarse desde 2 horas hasta 90 días antes del inicio.
* Una cancelación, tanto del cliente como del profesional, puede confirmarse hasta 2 horas antes del inicio.
* Dentro de las 2 horas previas, el sistema rechaza la cancelación y solicita contactar directamente a la otra parte.
* Los turnos pasados nunca pueden reservarse ni cancelarse.

### 2.4 Estados de turno

Los estados funcionales serán `confirmed`, `cancelled` y `no_show`.

`no_show` se incorporará mediante una Story nueva. Solo el profesional podrá marcarlo después de la hora de inicio. No modifica disponibilidad porque corresponde a un turno pasado.

### 2.5 Reserva para otra persona

Queda fuera del release 1.1. El nombre y correo ingresados deben pertenecer a la persona que asistirá. La delegación o representación se analizará en una Story futura.

### 2.6 Requisitos no funcionales

* **Disponibilidad:** 99,5 % mensual para frontend y API, excluyendo mantenimiento anunciado.
* **Rendimiento:** p95 menor o igual a 500 ms para APIs de lectura y p75 de LCP menor o igual a 2,5 s para login y página pública, medidos en condiciones controladas.
* **Concurrencia:** soportar 100 usuarios simultáneos y garantizar una sola reserva confirmada por profesional e instante.
* **Errores:** menos de 1 % de respuestas 5xx durante la prueba de capacidad.
* **Accesibilidad:** WCAG 2.2 nivel AA en los journeys críticos.
* **Compatibilidad:** últimas dos versiones mayores de Chrome, Edge, Firefox y Safari; anchos de referencia de 360, 768 y 1280 píxeles.
* **Seguridad:** OWASP ASVS nivel 1 como baseline; autorización verificada del lado servidor para todo endpoint autenticado.

## 3. Cuenta y activación del profesional

### CAQ-3 — Registro del profesional

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

### CAQ-4 — Inicio y cierre de sesión

* El logout invalida la sesión actual, limpia de memoria y caché todos los datos privados y redirige inmediatamente a `/login`.
* La invalidación se propaga a todas las pestañas del mismo navegador. Las sesiones de otros dispositivos permanecen activas; cerrar todas las sesiones será una acción separada futura.
* Cualquier ruta `/dashboard/*` sin sesión válida redirige a `/login` y no entrega datos privados desde la API.
* Campo faltante: `Ingresa tu correo y contraseña.` Credenciales incorrectas: `Email o contraseña incorrectos.`
* Después de 5 intentos fallidos dentro de 15 minutos, se bloquean nuevos intentos durante 15 minutos por combinación de cuenta e IP. La respuesta continúa siendo genérica para no revelar cuentas existentes.
* El renderizado observado después del logout se considera defecto hasta demostrar que no existe exposición. Desarrollo debe investigar routing, caché, renderizado y autorización de API; la causa no puede decidirse desde Producto.

### CAQ-5 — Recuperación de contraseña

* La nueva contraseña usa exactamente la política de CAQ-3 y no puede ser igual a la contraseña anterior.
* Una nueva solicitud invalida todos los enlaces de recuperación anteriores de esa cuenta.
* Se permiten hasta 5 solicitudes por correo y por IP en una hora. Al alcanzar el límite se conserva la respuesta no enumerativa y no se envía otro correo.
* Mensajes aprobados:
  * Solicitud aceptada o correo inexistente: `Si el email existe en nuestro sistema, recibirás un enlace para recuperar tu contraseña.`
  * Correo inválido: `Ingresa un correo electrónico válido.`
  * Token inválido, vencido o usado: `Este enlace ya no es válido. Solicita uno nuevo.`
* Después del cambio exitoso se invalidan las sesiones existentes y se redirige al login. No se inicia sesión automáticamente.

### CAQ-6 — Acceso a la URL pública

* El dashboard muestra una tarjeta permanente denominada `Mi enlace de reservas`, con la URL completa y acciones `Copiar enlace` y `Abrir página`.
* Después de copiar se muestra `Enlace copiado` mediante un mensaje accesible.
* El slug no es editable y no cambia al modificar el nombre durante el release 1.1.
* El dominio canónico del release es `https://cita-ai.vercel.app/`. La migración a `cita.ai` queda fuera de este release; cuando ocurra deberá mantener redirecciones permanentes durante al menos 12 meses.
* Antes de configurar disponibilidad, la página pública muestra el perfil y `No hay horarios disponibles por el momento.` El dashboard ofrece un enlace a configurar disponibilidad.
* La ausencia actual de la URL en el panel es una brecha de implementación, no un cambio de alcance.

## 4. Agenda, disponibilidad y gestión de turnos

### CAQ-11 — Configuración de disponibilidad semanal

* Los bloques se interpretan en la zona horaria del profesional.
* Se permiten bloques contiguos; no se consideran solapados.
* Un bloque no puede cruzar medianoche. Debe dividirse en dos días.
* Mensajes aprobados:
  * Fin no posterior al inicio: `La hora de fin debe ser posterior a la hora de inicio.`
  * Solapamiento: `Este horario se superpone con otro bloque del mismo día.`
  * Cruce de medianoche: `Divide el horario en dos bloques, uno para cada día.`
* El guardado es atómico: reemplaza toda la configuración o conserva íntegramente la anterior y muestra `No pudimos guardar tu disponibilidad. Intenta nuevamente.`
* Sesión QA 09/09/2026 (pendiente de ratificación PO): desactivar un día equivale a quitar sus intervalos dentro del reemplazo atómico; los turnos `confirmed` existentes no se cancelan y los slots futuros de ese día dejan de ofrecerse.
* Sesión QA 09/09/2026 (pendiente de ratificación PO): sin tope funcional de intervalos por día en este release; se renderizan en orden de inicio.
* Sesión QA 09/09/2026 (pendiente de ratificación PO): rige el texto aprobado para fin no posterior al inicio; la implementación observada (`La hora de inicio debe ser anterior a la de fin en Martes`) debe converger a él, o PO enmienda esta decisión.

### CAQ-12 — Duración estándar

* Los únicos valores válidos son 15, 30, 45, 60, 90 y 120 minutos. El valor inicial es 60 minutos.
* Un cambio afecta únicamente la generación de slots futuros. Nunca modifica la duración ni la hora de turnos ya creados.
* Si un bloque deja un remanente menor que la duración, el remanente se descarta y no genera un slot incompleto.
* La interfaz observada se adopta como regla del próximo release y reemplaza la regla histórica de cualquier entero positivo.

### CAQ-13 — Bloqueo de períodos

* El bloqueo se guarda y muestra en la zona del profesional; la API usa UTC.
* No se permite crear un bloqueo cuyo fin sea pasado ni cuyo fin sea igual o anterior al inicio.
* Se permiten bloqueos superpuestos; para disponibilidad se evalúa la unión de los intervalos.
* Un bloqueo puede cruzar medianoche porque utiliza fechas e instantes completos.
* Los turnos ya confirmados permanecen confirmados. Antes de guardar se muestra cuántos quedan dentro del bloqueo y se advierte que deben cancelarse manualmente si corresponde.
* El bloqueo impide únicamente nuevas reservas. Eliminarlo recalcula los slots que no estén ocupados por turnos.
* Sesión QA 09/09/2026 (pendiente de ratificación PO): el motivo admite hasta 250 caracteres, igual que el motivo de cancelación; excederlo rechaza el guardado sin persistir. El texto exacto del mensaje queda pendiente de observación.

### CAQ-14 — Registro manual de turno

* Se puede seleccionar un cliente existente o crear uno con nombre y correo bajo las reglas de CAQ-3.
* Se requieren cliente, fecha y hora futura. Se admite una nota opcional de hasta 250 caracteres.
* El profesional puede crear un turno fuera de su disponibilidad semanal como excepción manual.
* No puede crear un turno dentro de un bloqueo ni superpuesto con otro `confirmed`; primero debe eliminar el bloqueo o elegir otro horario.
* Si el correo ya pertenece a un cliente del profesional, se reutiliza el registro y no se cambia su nombre silenciosamente.
* La interfaz deshabilita el envío mientras procesa y el servidor usa una clave de idempotencia. Repetir la misma operación devuelve el turno ya creado.

### CAQ-15 — Consulta de próximos turnos

* Requiere sesión y autorización de servidor. Solo devuelve turnos del profesional autenticado.
* Cada fila muestra nombre y correo del cliente, fecha, hora y zona, estado y acciones disponibles.
* `Próximas Citas` contiene turnos futuros `confirmed`, ordenados de menor a mayor fecha.
* La vista pagina de a 20 turnos y permite filtrar por rango de fechas y buscar por nombre o correo.
* Los turnos `cancelled` quedan fuera de `Próximas Citas`; su historial se incorporará en una Story separada.
* El estado vacío observado se conserva con los textos actuales.
* Sesión QA 09/09/2026 (pendiente de ratificación PO): `Citas Hoy` cuenta turnos `confirmed` con inicio hoy en la zona del profesional; todo lo contado debe listarse en `Próximas Citas`. La divergencia observada el 09/09/2026 es defecto, no criterio.

## 5. Página pública y auto-reserva

### CAQ-16 — Consulta de horarios disponibles

* Fechas y horarios se presentan en la zona del profesional con una etiqueta visible.
* Solo se muestran slots que respeten la ventana de 2 horas a 90 días.
* Una semana sin slots muestra `No hay horarios disponibles esta semana. Prueba con otra fecha.`
* Un fallo al consultar muestra `No pudimos cargar los horarios. Intenta nuevamente.` y una acción de reintento.

### CAQ-17 — Página pública del profesional

* Los únicos datos públicos son nombre visible, zona horaria, duración estándar y slots disponibles.
* Correo del profesional, identificadores internos, configuración privada y datos de otros clientes nunca son públicos.
* Un slug inexistente responde HTTP 404 y muestra `No encontramos este perfil.`
* Un perfil válido sin disponibilidad mantiene la identidad del profesional y muestra `No hay horarios disponibles por el momento.`

### CAQ-18 — Confirmación sin cuenta

* Nombre y correo usan las mismas reglas de normalización y máximos de CAQ-3. No se solicita contraseña.
* La confirmación visible muestra `Tu turno fue reservado`, profesional, fecha, hora, zona y aviso de que el enlace de cancelación llegará por correo.
* El cliente no necesita aprobación posterior del profesional.
* La UI genera una clave de idempotencia por intento. Repetir durante 10 minutos la misma solicitud con esa clave devuelve el turno existente y no genera otro.
* Reservar para otra persona queda fuera de alcance según la sección 2.5.

### CAQ-19 — Conflicto concurrente

* La base debe garantizar como máximo un turno `confirmed` por profesional e instante. La garantía cubre reserva pública, alta manual y reintentos.
* Mensaje aprobado: `Este horario acaba de ser reservado por otra persona. Elige otro horario para continuar.`
* El mensaje recibe foco o se anuncia mediante una región accesible y la disponibilidad se actualiza inmediatamente.
* Nombre y correo se conservan en la pestaña hasta confirmar otro turno, cerrar la pestaña o abandonar la página pública.
* La validación no transaccional documentada debe reemplazarse o complementarse con una garantía atómica de persistencia.

## 6. Cancelaciones

### CAQ-20 — Cancelación por el cliente

* El enlace contiene un token aleatorio de al menos 256 bits; en la base se conserva solamente su hash.
* El token es válido desde la confirmación hasta el límite de cancelación de 2 horas antes del turno.
* Reenviar la confirmación reutiliza el mismo enlace mientras siga vigente. No existe regeneración autónoma del token en este release.
* Un turno ya cancelado muestra `Este turno ya fue cancelado.` sin repetir efectos.
* Token inválido: `El enlace de cancelación no es válido.`
* Turno pasado o dentro de la ventana restringida: `Ya no puedes cancelar este turno desde el enlace. Contacta al profesional.`
* Una cancelación válida persiste `cancelled` y libera el slot en una única operación antes de informar éxito.

### CAQ-21 — Cancelación por el profesional

* Aplica la ventana general de 2 horas.
* El motivo es opcional y admite hasta 250 caracteres. No se exigirá hasta que una necesidad posterior lo justifique.
* El diálogo de confirmación conserva el texto observado: `¿Estás seguro de que deseas cancelar esta cita? Esta acción no se puede deshacer.`
* Turno pasado o dentro de la ventana: `Ya no puedes cancelar este turno desde el panel. Contacta al cliente.`
* La autorización del servidor exige que el turno pertenezca al profesional autenticado.
* La API solo responde éxito después de persistir `cancelled` y liberar el slot. La UI no puede retirar el turno definitivamente ante una respuesta fallida.
* El comportamiento observado el 02/09/2026 es un defecto bloqueante. Desarrollo debe investigar la causa y si se intentó enviar un correo; Producto no presume una explicación ni un hecho que la evidencia no confirma.

## 7. Correos y recordatorios

### 7.1 Política común de correos de producto

* Resend es el proveedor para bienvenida, reserva, cancelación y recordatorio. Supabase se conserva para autenticación y recuperación.
* Cada evento funcional tiene un identificador idempotente para evitar correos duplicados.
* Ante fallo se realizan tres reintentos: después de 1 minuto, 10 minutos y 1 hora.
* Agotados los reintentos, el evento queda `failed`, se registra en monitoreo y se alerta al equipo. No se revierte una cuenta, reserva o cancelación ya persistida.
* La interfaz informa éxito de la operación principal y, cuando el usuario autenticado sea el actor, advierte `La operación se completó, pero no pudimos enviar el correo.`

### CAQ-22 — Aviso de cancelación

* Si cancela el cliente se notifica solo al profesional; si cancela el profesional se notifica solo al cliente.
* El aviso incluye quién canceló, nombre del profesional, fecha, hora, zona horaria y estado `Cancelado`.
* El correo se genera únicamente después de persistir la cancelación.
* Una falla de entrega no revierte la cancelación y usa la política común de reintentos.

### CAQ-23 — Confirmación de reserva

* El cliente recibe nombre del profesional, fecha, hora, zona, estado y enlace de cancelación.
* El profesional recibe nombre y correo del cliente, fecha, hora, zona y origen `Reserva pública`.
* El turno se confirma antes de generar los correos. Una falla de entrega no revierte la reserva.
* Ambos envíos aplican la política común de reintentos y deduplicación.

### CAQ-24 — Recordatorio

* El recordatorio se incorpora al release 1.1.
* Se envía al cliente exactamente 24 horas antes del turno `confirmed`. El scheduler opera en UTC y el contenido usa la zona del profesional.
* Un turno creado con menos de 24 horas de anticipación no recibe recordatorio.
* Incluye profesional, fecha, hora, zona y enlace de cancelación si todavía está dentro de la ventana permitida.
* Turnos `cancelled` y `no_show` no generan recordatorios.
* El identificador del turno y el tipo `reminder-24h` forman la clave de deduplicación. Se aplica la política común de tres reintentos.

## 8. Clientes y límite freemium

### CAQ-25 — Listado de clientes

* El correo se normaliza con trim y minúsculas para determinar unicidad dentro de cada profesional.
* El listado se ordena alfabéticamente por nombre y, ante empate, por correo.
* Pagina de a 20 y permite búsqueda parcial por nombre o correo.
* El historial de turnos queda fuera de CAQ-25 y requiere una Story separada.
* Solo se muestran clientes asociados con el profesional autenticado.

### CAQ-26 — Carga manual de cliente

* `Nuevo Cliente` debe abrir un formulario con nombre y correo bajo las reglas de CAQ-3. La falta de respuesta observada se considera defecto.
* Si el correo ya existe para el profesional, no se crea ni actualiza otro registro. Se muestra `Este cliente ya existe en tu listado.` y se ofrece abrirlo.
* Al alcanzar el límite se muestra `Alcanzaste el límite de 10 clientes del plan gratuito. Tus clientes actuales pueden seguir reservando.`
* Un alta válida aparece inmediatamente en el listado respetando su orden.

### CAQ-27 — Límite de diez clientes únicos

* El conteo usa el correo normalizado por profesional.
* Un cliente comienza a contar cuando se crea manualmente o cuando obtiene su primer turno `confirmed`.
* Cancelar turnos no reduce el conteo. El release 1.1 no incluye eliminación de clientes; una futura baja deberá definir su efecto antes de implementarse.
* Los clientes existentes pueden reservar turnos ilimitados.
* Mensaje público para el cliente número once: `Este profesional alcanzó el límite de nuevos clientes. Contacta directamente al profesional para coordinar tu turno.`
* El bloqueo se aplica de forma consistente a reserva pública y alta manual.

### CAQ-28 — Información sobre Plan Pro

* Plan Pro no tendrá precio, beneficios, cobro ni contratación en el release 1.1. La acción solo registra interés.
* Al alcanzar diez clientes se envía una sola vez el correo: `¡Tu agenda está creciendo! Alcanzaste los 10 clientes de tu plan gratuito. Tus clientes actuales pueden seguir reservando. Si quieres conocer futuras opciones, registra tu interés en el Plan Pro.`
* El panel muestra permanentemente: `Alcanzaste el límite de 10 clientes. Tus clientes actuales pueden seguir reservando.` y la acción `Más información sobre el Plan Pro`.
* La acción registra `professional_id`, fecha UTC, origen y versión del mensaje. No registra datos de pago.
* La operación es idempotente. Después de ejecutarla, la acción se reemplaza por `Interés registrado`; el aviso permanece visible.
* El correo no vuelve a enviarse si el conteo baja y alcanza nuevamente el límite.

## 9. Entornos, datos y operación del release

### 9.1 Entorno de QA

* Antes de ejecutar pruebas mutantes debe crearse un entorno `QA` aislado con frontend Vercel, proyecto Supabase y configuración de correo propios.
* No se reactivará el UAT histórico hasta completar un inventario y eliminación verificable de PII. Si no puede acreditarse su limpieza, se descarta y se crea un entorno nuevo.
* Producción queda limitada a smoke tests de solo lectura y verificaciones expresamente aprobadas.
* QA no tendrá acceso directo de escritura a la base productiva. En QA se utilizará un rol restringido y auditado.

### 9.2 Datos de prueba

* QA Lead será responsable funcional de fixtures y teardown; Tech Lead será responsable técnico de seed, factories y migraciones.
* Los datos usarán un `run_id` y se eliminarán dentro de las 24 horas posteriores a la ejecución.
* Se crearán profesionales y clientes totalmente sintéticos; nunca se copiarán datos productivos.
* Resend se configurará en modo sandbox o con una lista cerrada de destinatarios del equipo.
* La cuenta profesional compartida se custodiará en el gestor de secretos corporativo. Las automatizaciones leerán referencias desde configuración ignorada o variables protegidas.
* La política de retención será: datos funcionales de pruebas, 24 horas; logs QA, 30 días; evidencias sin PII, durante el curso y 90 días adicionales.

### 9.3 Calidad de entrega y operación

* El repositorio de aplicación debe documentar el arranque local de Next.js y Supabase antes de considerar una Story `Ready for QA`. Tech Lead es responsable de proporcionar comandos reproducibles.
* Todo cambio debe pasar pull request, lint, pruebas unitarias, integración y smoke test antes del despliegue. No se permite despliegue automático a producción desde un push sin estas validaciones.
* Los cambios de schema requieren migración versionada y revisión de Tech Lead. Producto aprueba el impacto funcional, no la implementación SQL.
* Producción tendrá backup diario con retención de 7 días, **RPO de 24 horas** y **RTO de 4 horas**.
* Se monitorearán disponibilidad, respuestas 5xx, errores de autenticación, fallos de jobs y estado de correos. Una alerta crítica debe notificarse en menos de 5 minutos.

## 10. Trabajo derivado obligatorio

Antes de declarar las Stories listas para implementación deben crearse o enlazarse los siguientes trabajos:

1. Bug por cancelación con falso éxito y slot no liberado de CAQ-21.
2. Investigación de autorización y caché posterior al logout de CAQ-4.
3. Story para configuración de zona horaria del profesional.
4. Story para estado `no_show`.
5. Story para historial de turnos y clientes, fuera del listado actual.
6. Trabajo de infraestructura para entorno QA, seed, teardown y correo sandbox.
7. Trabajo técnico para garantía atómica de reserva e idempotencia.
8. Trabajo operativo para pipeline, migraciones, monitoreo y backups.

## 11. Criterio para cerrar las preguntas abiertas

Al reprocesar el backlog:

* Cada respuesta de este documento debe trasladarse a la Story y a sus escenarios Gherkin.
* La tabla de Fuentes debe citar este archivo y la sección correspondiente.
* La pregunta respondida se elimina de `Preguntas abiertas`.
* Las investigaciones sobre hechos pasados permanecen abiertas hasta contar con evidencia técnica; se reemplazan por el comportamiento esperado aquí aprobado.
* Luego se repite la inspección Shift-Left. Una Story solo queda `Aprobado` si no conserva defectos bloqueantes ni criterios sin resultado verificable.

## Fuentes consideradas

| Fuente | Uso |
| :--- | :--- |
| `.context/PBI/epic-tree.md` | Preguntas y contradicciones consolidadas del backlog |
| `.context/PBI/epics/` | Preguntas, fuentes y escenarios de cada Story |
| `.context/testing/inspections/` | Defectos y decisiones pendientes detectados por QA |
| `.context/testing/test-plan-*.md` | Riesgos, entornos y datos necesarios |
| `.context/architecture/prd.md` | Alcance y comportamiento funcional reconstruido |
| `.context/infrastructure/environments.md` | Estado vigente de entornos y operación |
| `.context/infrastructure/test-data-strategy.md` | Restricciones y necesidades de datos de prueba |
| `.context/Confluence-corporativo/` | Evidencia histórica inmutable; no fue modificada |
