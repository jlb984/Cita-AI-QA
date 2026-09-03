# Story: Como parte de un turno cancelado, quiero recibir un aviso, para conocer el cambio

**ID:** CAQ-22
**Epic:** CAQ-9
**Implementación:** Sin verificar
**Modo de exploración:** Navegador automatizado
**Entorno observado:** Producción · 02/09/2026
**Estado de sincronización:** Sincronizado

## Descripción

Como parte de un turno cancelado, quiero recibir un aviso, para conocer el cambio.

## Criterios de Aceptación (Borrador)

- [ ] Cuando cancela el cliente, el sistema envía un aviso al profesional.
- [ ] Cuando cancela el profesional, el sistema envía un aviso al cliente.
- [ ] El aviso identifica quién canceló.
- [ ] No se envía el aviso de cancelación a la misma parte que inició la acción.
- [ ] Una falla de entrega no debe revertir silenciosamente el estado ya cancelado del turno.

## Comportamiento observado

| Qué hace | Evidencia | Qué decía la documentación |
| :--- | :--- | :--- |
| No se disparó ningún aviso: verificarlo exige cancelar un turno y enviar un correo real desde producción, única instancia operativa documentada. | Sin evidencia de ejecución; recorrido detenido antes de modificar datos o enviar correos. | `.context/infrastructure/environments.md` · Mapa de Entornos y Riesgos; `.context/infrastructure/test-data-strategy.md` · Limpieza y Reset |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Destinatario y contenido del aviso | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 6.3 y 7 |
| Existencia actual del correo de cancelación | `.context/Confluence-corporativo/05-hilo-mail-cambio-de-alcance.md` · correo del 28/02/2026 |
| Un profesional reportó no haber visto el aviso; soporte confirmó que estaba en spam | `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · Cancelaciones, ticket #55 |
| Manejo de una falla de correo posterior a cancelar | **Hipótesis** — la documentación indica envíos sincrónicos, pero no define atomicidad entre estado y notificación |
| Producción es el único entorno operativo y los correos salen realmente | `.context/Confluence-corporativo/documentacion para QA/nota-ambientes-y-accesos.md` · Lo del ambiente de UAT y Cómo entrar |

## Contradicciones detectadas

* La especificación funcional y las notas técnicas iniciales atribuyen los correos al servicio de Supabase; el hilo del 28/02/2026 documenta una migración posterior de los correos de producto a Resend. Se toma el hilo más reciente para el proveedor actual, sin alterar los archivos históricos.

## Preguntas abiertas

* ¿Cuál es el asunto y el cuerpo literal del aviso para cada parte que cancela?
* ¿Cómo identifica el mensaje quién canceló y qué datos del turno incluye?
* ¿Qué estado conserva el turno cuando Resend rechaza, demora o no entrega el correo?
* ¿Existe reintento, trazabilidad o alerta ante fallas de entrega?
* ¿Qué entorno aislado e interceptor de correo permiten comprobar ambos sentidos del aviso sin notificar a personas reales?
