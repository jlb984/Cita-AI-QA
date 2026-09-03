# Story: Como cliente final, quiero cancelar mediante el enlace de mi correo, para liberar el horario sin crear una cuenta

**ID:** CAQ-20
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Modo de exploración:** Navegador automatizado
**Entorno observado:** Producción · 02/09/2026
**Estado de sincronización:** Sincronizado

## Descripción

Como cliente final, quiero cancelar mediante el enlace de mi correo, para liberar el horario sin crear una cuenta.

## Criterios de Aceptación (Borrador)

- [ ] El correo de confirmación incluye un enlace único de cancelación.
- [ ] El enlace permite cancelar sin iniciar sesión.
- [ ] Solo se puede cancelar un turno que no esté en el pasado.
- [ ] La cancelación cambia el estado del turno a `cancelled`.
- [ ] El horario cancelado vuelve a estar disponible.

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| No se recorrió la cancelación pública: no se dispuso de un enlace de cancelación perteneciente a datos sintéticos y generar uno exigiría crear un turno y enviar correos reales en producción. | Sin evidencia de ejecución; recorrido detenido antes de modificar datos. | `.context/infrastructure/environments.md` · Mapa de Entornos y Riesgos; `.context/infrastructure/test-data-strategy.md` · Limpieza y Reset |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Enlace único sin cuenta | `.context/Confluence-corporativo/01-minuta-kickoff.md` · Los dos usuarios del sistema; `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.1 |
| Restricción temporal, estado y liberación | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 6.3 |
| Riesgo del endpoint público | `.context/Confluence-corporativo/04-notas-tecnicas.md` · Endpoints |
| Tres enlaces de cancelación fallaron durante marzo de 2026 y no hubo nuevos reportes después de fin de ese mes | `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · Cancelaciones; `.context/Confluence-corporativo/documentacion para QA/transcripcion-reunion-2026-05-19.md` · 00:02:24–00:02:56 |
| Producción es el único entorno operativo y no admite rollback de datos | `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · Lo del ambiente de UAT |

## Contradicciones detectadas

* La especificación funcional afirma que el enlace lleva un identificador único del turno; las notas técnicas advierten que el endpoint público usa el `id` del turno con `service role` y que quien adivine ese identificador podría cancelar. No se elige entre ambas versiones: la seguridad actual del enlace requiere verificación.
* Los tickets de soporte registran tres errores de enlace durante marzo de 2026; la reunión del 19/05/2026 afirma que el problema fue corregido y no volvió a reportarse. No se pudo comprobar el estado actual sin un enlace sintético seguro.

## Preguntas abiertas

* ¿El enlace público utiliza actualmente un token impredecible o expone el `id` del turno?
* ¿Qué textos literales se muestran antes de confirmar, después de cancelar y al abrir un enlace inválido, ya utilizado o correspondiente a un turno pasado?
* ¿La cancelación pública es idempotente ante reintentos?
* ¿El horario vuelve a publicarse inmediatamente y se envía el aviso al profesional cuando la entrega de correo falla?
* ¿Qué entorno aislado y qué datos sintéticos permiten recorrer el flujo completo sin afectar producción?
