# Análisis de Escenarios: Cancelación de un turno por el profesional

**Historia:** CAQ-21  
**Fecha:** 2026-09-09  
**Implementación de la historia:** Parcial  
**Sesiones analizadas:**

- `.context/testing/exploratory/ui/session-2026-09-07-cancelacion-por-profesional.md`
- `.context/testing/exploratory/ui/session-2026-09-07-caq21-cancelacion.md`

La historia está marcada como `Parcial`. Los candidatos se diseñan contra comportamiento existente, pero la persistencia y la liberación del slot tienen un defecto crítico documentado. La sesión `session-2026-09-07-cancelacion-por-profesional.md` informa que la ventana de dos horas fue ejecutada y falló; la sesión `session-2026-09-07-caq21-cancelacion.md` la registra como no ejecutada por falta de datos. Se conserva la evidencia de la ejecución fallida y se deja asentada la discrepancia.

## Candidatos para Regresión

| # | Tipo | Escenario | Origen | Estado en la sesión |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Funcional / Persistencia | Cancelar un turno futuro propio: el estado pasa a `cancelled`, el turno desaparece de próximas citas y la operación persiste después de una navegación nueva | Sesiones UI de CAQ-21 · Escenarios 1 y 6 | Ejecutado, FAIL (`CAQ-29`) |
| 2 | Funcional / Disponibilidad | Liberar el horario público después de cancelar un turno propio | Sesión UI `session-2026-09-07-caq21-cancelacion.md` · Escenario 7 | Ejecutado, FAIL (`CAQ-29`) |
| 3 | Funcional / Interfaz | Mostrar el texto exacto de confirmación y no exigir un motivo al iniciar la cancelación de un turno futuro | Sesión UI `session-2026-09-07-caq21-cancelacion.md` · Escenario 4 | Ejecutado, PASS |
| 4 | Funcional / Interfaz | Rechazar la cancelación en el diálogo y conservar el turno sin modificarlo | Sesión UI `session-2026-09-07-cancelacion-por-profesional.md` · Escenario 2 | Ejecutado, PASS |
| 5 | Regla de negocio | Rechazar la cancelación cuando faltan menos de dos horas y mostrar `Ya no puedes cancelar este turno desde el panel. Contacta al cliente.` | Sesión UI `session-2026-09-07-cancelacion-por-profesional.md` · Escenario 3; CAQ-21 · Escenario 5 | Ejecutado, FAIL (`CAQ-21-BUG-02`) |
| 6 | Robustez / Error de API | Ante una persistencia fallida, no informar éxito ni retirar definitivamente el turno confirmado de la interfaz | Sesiones UI de CAQ-21 · comportamiento posterior a los Escenarios 1 y 6 | Ejecutado, FAIL (`CAQ-29`) |
| 7 | Seguridad / Autorización | Impedir que un profesional cancele un turno perteneciente a otro profesional, sin cambiar estado ni disponibilidad | `story.md` · CA-2; sesión UI `session-2026-09-07-caq21-cancelacion.md` · Escenario 9 | No ejecutado |
| 8 | Regla de negocio | Impedir la cancelación de un turno pasado y conservar su estado | `story.md` · CA-3; sesión UI `session-2026-09-07-caq21-cancelacion.md` · Escenario 10 | No ejecutado |
| 9 | Funcional / Datos | Registrar un motivo opcional de hasta 250 caracteres y permitir la cancelación sin motivo | `story.md` · CA-6; sesión UI `session-2026-09-07-caq21-cancelacion.md` · Sin cobertura | No ejecutado; la UI no ofrece campo de motivo |

Los casos 1, 2 y 6 forman un único riesgo funcional observado desde tres verificaciones: la UI muestra un retiro optimista, pero la API conserva `confirmed` y el horario no vuelve a estar disponible. Deben mantenerse separados en la regresión porque comprueban persistencia, disponibilidad y manejo visible del error.

## Huecos de Cobertura

- **CA-2, turno ajeno:** ninguna sesión contó con un segundo profesional o un identificador preparado para comprobar autorización del servidor y ausencia de cambios en estado y disponibilidad.
- **CA-3, turno pasado:** no había un turno pasado asociado a la cuenta de prueba. Hace falta un fixture o dato sintético controlado que permita ejecutar la operación sin depender de datos productivos.
- **CA-6, motivo opcional:** la UI no muestra un campo de motivo. No se pudo verificar ni el límite de 250 caracteres ni la persistencia del motivo; hace falta que la interfaz o la API exponga el contrato verificable.
- La sesión `session-2026-09-07-caq21-cancelacion.md` dejó sin ejecutar la ventana de dos horas por falta de un turno adecuado, pero la otra sesión sí la ejecutó y observó que la restricción no se aplica. Debe repetirse con un dato controlado después de corregir `CAQ-21-BUG-02`.

## Descartados

- La creación del turno sintético y la comprobación del estado inicial del dashboard se usaron como preparación de la sesión, no como escenarios de regresión de CAQ-21.
- La prueba de doble clic o clics rápidos y la visualización responsive a 375 px fueron verificaciones exploratorias de robustez de UI. No corresponden a un criterio de aceptación de CAQ-21 ni descubrieron una desviación que justifique un caso independiente.
- El contador de citas de hoy inconsistente y la oferta pública de un slot ocupado pertenecen a CAQ-15 y a disponibilidad pública, respectivamente. Se conservaron como hallazgos de sus historias de origen y no se atribuyen a CAQ-21.
- La doble reserva del slot y los correos de cancelación quedaron fuera del alcance de estas sesiones y requieren condiciones de prueba aisladas; no se presentan como resultados de CAQ-21.

## Trazabilidad

| Historia (US) | Caso / escenario | Bug | Evidencia |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Cancelación de turno futuro propio y persistencia | CAQ-29 | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-reaparece-tras-recarga-fail.png` |
| CAQ-21 | Liberación del horario después de cancelar | CAQ-29 | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-slot-no-liberado-o-disponible-fail.png` |
| CAQ-21 | Confirmación con texto exacto y sin motivo obligatorio | — | `.context/testing/exploratory/ui/session-2026-09-07-caq21-cancelacion.md` · Escenario 4 |
| CAQ-21 | Rechazo en el diálogo de confirmación | — | `.context/testing/exploratory/ui/session-2026-09-07-cancelacion-por-profesional.md` · Escenario 2 |
| CAQ-21 | Restricción de cancelación dentro de dos horas | CAQ-21-BUG-02 | `.context/testing/exploratory/ui/session-2026-09-07-cancelacion-por-profesional.md` · Escenario 3 |
| CAQ-21 | Conservación de la UI ante fallo de persistencia | CAQ-29 | `.context/testing/exploratory/ui/evidence/screenshots/2026-09-07-caq21-turno-retirado-tras-confirmar-pass.png` |
| CAQ-21 | Impedir cancelar turno ajeno | — | Sin evidencia: no ejecutado |
| CAQ-21 | Impedir cancelar turno pasado | — | Sin evidencia: no ejecutado |
| CAQ-21 | Motivo opcional de hasta 250 caracteres | — | Sin evidencia: no ejecutado; no existe campo visible |

## Sin cobertura

- No se verificó la autorización contra un turno ajeno ni la conservación de su estado y disponibilidad.
- No se verificó la protección de un turno pasado.
- No se verificó el motivo opcional, su límite de 250 caracteres ni su persistencia.
- No se confirmó la causa técnica del falso éxito ni si se intentó enviar el correo de cancelación; requiere trazas del backend y un buzón o sandbox controlado.
- No se ejecutó la doble reserva del horario ocupado porque la sesión se realizó en producción y crear más datos no estaba autorizado.

Se necesitan datos de prueba aislados o fixtures controlados para completar estos casos sin depender de reservas productivas ni de correos reales.

Se sugiere priorizar estos candidatos con `.prompts/7-Documentacion CPs/test-prioritization.md`.
