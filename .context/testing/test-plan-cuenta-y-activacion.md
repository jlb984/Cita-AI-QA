# Plan de Pruebas: Cuenta y activación del profesional

**Epic:** CAQ-2
**Fecha:** 04/09/2026

## 1. Matriz de Riesgos del Producto

| ID | Riesgo | Probabilidad (1-5) | Impacto (1-5) | Nivel | Mitigación |
| :--- | :--- | :---: | :---: | :--- | :--- |
| R1 | Acceso a datos después del logout | 4 | 5 | 20 (Alto) | Integración de autorización y E2E con dos cuentas |
| R2 | Registro parcial sin perfil, slug o sesión recuperable | 3 | 4 | 12 (Medio) | Pruebas de fallos parciales e idempotencia |
| R3 | Enumeración o abuso de recuperación | 3 | 5 | 15 (Alto) | Seguridad de mensajes, tokens y rate limit |
| R4 | URL pública no localizable | 4 | 4 | 16 (Alto) | E2E de activación y acceso al enlace |

## 2. Niveles de Prueba (Pyramid)

* **Unitarias:** Jest para validaciones, normalización y generación/colisión de slug.
* **Integración:** Postman/Newman para Supabase Auth, cookies, middleware, tokens y persistencia de perfil.
* **E2E:** Playwright para registro, login, recuperación, logout y localización de URL.

## 3. Pruebas No Funcionales

* **Seguridad:** OWASP ZAP y pruebas dirigidas de enumeración, fijación/revocación de sesión, autorización y abuso de tokens.
* **Performance:** k6 para p95 de lecturas menor o igual a 500 ms y LCP p75 menor o igual a 2,5 s en login.
* **Accesibilidad:** axe con Playwright y revisión manual WCAG 2.2 AA para formularios, errores y foco.

## 4. Necesidades de Entorno y Datos

| Necesidad | Entorno | ¿Disponible hoy? | Referencia |
| :--- | :--- | :--- | :--- |
| Dos profesionales sintéticos y buzones controlados | Producción | No | `test-data-strategy.md` · Gestión de Usuarios |
| Control de tokens, reloj y fallos de correo | Producción | No | `test-data-strategy.md` · Fuentes de Datos |
| Exploración de solo lectura con cuenta de prueba | Producción | Parcial | `environments.md` · Detalles de Acceso |
| Entorno aislado con Supabase y correo propios | QA requerido por Producto | No | `decisiones-po-proximo-release.md` · 9.1; no figura activo en `environments.md` |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Riesgos funcionales | `.context/PBI/epics/EPIC-CAQ-2-cuenta-y-activacion-del-profesional/epic.md` y sus Stories |
| Entorno único | `.context/infrastructure/environments.md` · Mapa de Entornos |
| Datos mutantes no disponibles | `.context/infrastructure/test-data-strategy.md` · Fuentes de Datos |
| Reglas y objetivos vigentes | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-3, CAQ-4 a CAQ-6 y 2.6 |
| Probabilidad, impacto y nivel | **Hipótesis** — valoración de riesgo de QA |

## Contradicciones detectadas

* El dashboard debe quedar protegido, pero se observó renderizado tras logout.
* Producto exige un entorno QA aislado; el mapa vigente solo confirma Producción.

## Preguntas abiertas

* ¿Cuál es la causa técnica del renderizado posterior al logout?
* ¿La tarjeta «Mi enlace de reservas» continúa ausente en el build que se entregará a QA?
* ¿Cuándo estarán disponibles el entorno QA y sus datos sintéticos?
