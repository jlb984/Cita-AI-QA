# Epic: [Epic] Cuenta y activación del profesional

**ID:** CAQ-2
**Estado de sincronización:** Sincronizado con Jira (`CAQ`)
**Estado:** To Do

## Descripción

Permitir que un profesional cree su cuenta, acceda de manera segura, recupere sus credenciales y obtenga la URL pública que necesita para comenzar a recibir reservas. El alcance proviene de `.context/architecture/prd.md` · Feature 1 y de `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · sección 3.

## User Stories

- [ ] CAQ-3: Como profesional, quiero registrarme, para comenzar a configurar mi agenda.
- [ ] CAQ-4: Como profesional, quiero iniciar y cerrar sesión, para acceder de forma segura a mi panel.
- [ ] CAQ-5: Como profesional, quiero recuperar mi contraseña, para volver a acceder sin revelar si mi correo está registrado.
- [ ] CAQ-6: Como profesional, quiero encontrar mi URL pública, para compartirla con mis clientes.

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| El profesional se registra con nombre, correo y contraseña | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 2.1 y 3.1 |
| La activación incluye sesión automática, configuración inicial y URL pública | `.context/architecture/prd.md` · Feature 1 y User Journeys |
| El acceso incluye inicio de sesión y recuperación de contraseña | `.context/Confluence-corporativo/03-especificacion-funcional-v0.3.md` · secciones 3.2 y 3.3 |
| El profesional necesita localizar y compartir su URL | `.context/Confluence-corporativo/06-tickets-soporte-resumen.md` · Registro y acceso; `.context/Confluence-corporativo/documentacion para QA/transcripcion-reunion-2026-05-19.md` · 00:01:17–00:02:20 |
| Invalidar el acceso a rutas protegidas al cerrar sesión | **Hipótesis** — el middleware está documentado, pero el comportamiento esperado posterior al logout figura como pregunta abierta en el PRD |
