# Story: Como profesional, quiero cancelar un turno desde mi panel, para actualizar mi agenda

**ID:** CAQ-21
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Modo de exploración:** Navegador automatizado
**Entorno observado:** Producción · 02/09/2026
**Estado de sincronización:** Sincronizado

## Descripción

Como profesional, quiero cancelar un turno desde mi panel, para actualizar mi agenda.

## Criterios de Aceptación (Borrador)

- [ ] El profesional autenticado puede iniciar la cancelación desde un turno de su agenda.
- [ ] El profesional solo puede cancelar turnos asociados con su cuenta.
- [ ] No se puede cancelar un turno pasado.
- [ ] La cancelación cambia el estado a `cancelled` y libera el horario.
- [ ] El motivo de cancelación no se exige mientras negocio no defina esa regla.

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| Al abrir `/dashboard` sin una sesión autenticada, la pantalla muestra «No tienes citas próximas» y no presenta un turno desde el cual iniciar la cancelación. El camino no pudo continuar porque faltan credenciales de prueba y cancelar en el único entorno disponible modificaría producción. | `evidence/2026-09-02-dashboard-sin-citas-ni-sesion.png` | La especificación indica que el profesional cancela desde su panel; la documentación de infraestructura registra que las rutas privadas pueden renderizar después del logout. |
| Con sesión autenticada, `/dashboard` muestra las citas futuras dentro de «Próximas Citas». Cada cita presenta el nombre del cliente, fecha, hora y las acciones «Contactar» y «Cancelar». | `evidence/2026-09-02-dashboard-citas-con-accion-cancelar.png` | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 2.1 y 6.2: el profesional puede cancelar un turno desde su panel. |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Cancelación desde el panel | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 2.1 y 6.2 |
| Aislamiento por profesional | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Row level security |
| Restricción temporal, estado y liberación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3 |
| El dashboard sin sesión muestra cero citas próximas y no ofrece un turno desde el cual cancelar | **Observado** — producción, 02/09/2026. Evidencia: `evidence/2026-09-02-dashboard-sin-citas-ni-sesion.png` |
| Las citas futuras muestran nombre del cliente, fecha, hora y las acciones «Contactar» y «Cancelar» | **Observado** — producción, 02/09/2026. Evidencia: `evidence/2026-09-02-dashboard-citas-con-accion-cancelar.png` |
| Motivo de cancelación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3, marcado `TBD` |
| Producción es el único entorno operativo y no admite rollback de datos | `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · Lo del ambiente de UAT |

## Contradicciones detectadas

* La documentación describe el panel como autenticado y protegido por middleware, pero `/dashboard` renderizó sin sesión. La captura no permite determinar si las APIs de cancelación también carecen de autorización; ese hallazgo de acceso no se convierte en comportamiento esperado de esta historia.

## Preguntas abiertas

* ¿Pulsar «Cancelar» abre una confirmación previa o ejecuta la cancelación inmediatamente, y qué textos literales presenta?
* ¿Qué mensaje aparece después de completar la acción y a qué pantalla redirige?
* ¿Debe el profesional informar un motivo y, en ese caso, se comunica al cliente?
* ¿Cómo responde la interfaz ante turnos pasados, ya cancelados o pertenecientes a otro profesional?
* ¿El horario se libera inmediatamente y qué ocurre si falla el aviso por correo?
* ¿Qué entorno aislado, usuario de prueba y turno sintético permiten completar el recorrido sin afectar producción?
