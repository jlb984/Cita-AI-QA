# Priorización de Pruebas: Cancelación de un turno por el profesional

**Historia:** CAQ-21  
**Fecha:** 2026-09-09  
**Matriz de riesgos usada:** `.context/testing/test-plan-cancelaciones-y-comunicaciones.md`

La historia tiene implementación `Parcial`. Los escenarios que ya fallaron se priorizan por el riesgo que mitigan y por su repetición esperada en cada release. Los escenarios no ejecutados conservan la decisión `PROBAR ANTES DE DECIDIR`, tal como exige el criterio de priorización.

## Matriz de Priorización

| # | Escenario | Frec. | Crit. | Compl. | Score | Decisión | Justificación |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- | :--- |
| 1 | Cancelar un turno futuro propio, persistir `cancelled` y retirarlo después de una navegación nueva | 1 | 1 | 0.8 | 2.8 | AUTOMATIZAR | Se ejecutará en cada release y mitiga `R1` (alto): cancelación que aparenta éxito sin persistir. La verificación UI + API + recarga es estable y repetible en un entorno con datos controlados. |
| 2 | Liberar el horario público después de cancelar un turno propio | 1 | 1 | 0.8 | 2.8 | AUTOMATIZAR | Se ejecutará en cada release y mitiga `R2` (alto): el slot cancelado no vuelve a ofrecerse. Requiere una aserción de disponibilidad posterior a la cancelación. |
| 3 | Mostrar el texto exacto de confirmación y no exigir un motivo al iniciar la cancelación | 1 | 0 | 1 | 2.0 | MANUAL | Es frecuente y sencillo de comprobar, pero la matriz no vincula este detalle de interfaz con un riesgo alto. Mantenerlo manual permite cubrir el contrato visible sin invertir primero en automatización. |
| 4 | Rechazar la cancelación en el diálogo y conservar el turno sin modificarlo | 1 | 0 | 0.9 | 1.9 | MANUAL | Es un control frecuente y estable de la interfaz, sin riesgo alto identificado en la matriz. Puede incorporarse a una regresión manual corta. |
| 5 | Rechazar la cancelación cuando faltan menos de dos horas y mostrar el mensaje de restricción | 1 | 0 | 0.8 | 1.8 | MANUAL | La ventana temporal se revisará en cada release, pero el plan no le asigna un riesgo de matriz específico. La dependencia de datos temporales y reloj hace conveniente mantenerla manual hasta contar con control del tiempo. |
| 6 | Ante un fallo de persistencia, no informar éxito ni retirar definitivamente el turno confirmado | 1 | 1 | 0.7 | 2.7 | AUTOMATIZAR | Mitiga `R1` (alto), que ya se reprodujo como defecto crítico. La prueba requiere simular o provocar la respuesta fallida y comprobar que la UI conserva el estado; su complejidad es mayor, pero el impacto justifica automatizarla. |
| 7 | Impedir que un profesional cancele un turno perteneciente a otro profesional | — | — | — | — | PROBAR ANTES DE DECIDIR | Hueco de cobertura. Mitiga `R3` (alto), pero todavía falta ejecutar el caso con datos de dos profesionales antes de decidir su automatización. |
| 8 | Impedir la cancelación de un turno pasado y conservar su estado | — | — | — | — | PROBAR ANTES DE DECIDIR | Hueco de cobertura. No hay evidencia de ejecución ni datos controlados para puntuar frecuencia, criticidad y complejidad. |
| 9 | Registrar un motivo opcional de hasta 250 caracteres y permitir la cancelación sin motivo | — | — | — | — | PROBAR ANTES DE DECIDIR | Hueco de cobertura. La UI observada no ofrece campo de motivo; primero hay que confirmar el contrato implementado y probarlo una vez. |

## Resumen

- **Candidatos a automatización:** 3
- **Regresión manual:** 3
- **Probar antes de decidir:** 3
- **Descartados:** 0 de los candidatos analizados

Los controles exploratorios de doble clic, responsive, creación del turno y estado inicial fueron descartados en el análisis de escenarios por no constituir candidatos propios de CAQ-21 o por no mitigar un riesgo priorizado.

## Trazabilidad

| Historia (US) | Caso / escenario | Riesgo mitigado | Origen |
| :--- | :--- | :--- | :--- |
| CAQ-21 | Persistencia de la cancelación del turno futuro propio | `R1` | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 1 |
| CAQ-21 | Liberación del horario público | `R2` | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 2 |
| CAQ-21 | Confirmación con texto exacto y sin motivo obligatorio | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 3 |
| CAQ-21 | Rechazo en el diálogo de confirmación | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 4 |
| CAQ-21 | Restricción de cancelación dentro de dos horas | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 5 |
| CAQ-21 | Conservación de la UI ante fallo de persistencia | `R1` | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 6 |
| CAQ-21 | Impedir cancelar turno ajeno | `R3` | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 7 |
| CAQ-21 | Impedir cancelar turno pasado | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 8 |
| CAQ-21 | Motivo opcional de hasta 250 caracteres | — | `.context/testing/documentation/CAQ-21/analisis-escenarios.md` · caso 9 |

## Sin cobertura

- No se pudo puntuar la autorización contra un turno ajeno porque no existe evidencia de ejecución con dos profesionales; primero debe prepararse un fixture o entorno aislado.
- No se pudo puntuar la protección de turnos pasados porque faltan datos controlados y una estrategia para manejar el reloj.
- No se pudo puntuar el motivo opcional porque la UI no muestra el campo y el contrato de persistencia no está verificado.
- La matriz de riesgos no asigna un riesgo específico a la confirmación, al rechazo del diálogo ni a la ventana de dos horas; su criticidad se dejó en `0` en lugar de inventar un riesgo. Esto no bloquea su cobertura manual.

Se sugiere documentar los casos seleccionados con `.prompts/7-Documentacion CPs/test-documentation.md`.
