# Plan de Pruebas: Clientes y límite freemium

**Epic:** CAQ-10
**Fecha:** 04/09/2026

## 1. Matriz de Riesgos del Producto

| ID | Riesgo | Probabilidad (1-5) | Impacto (1-5) | Nivel | Mitigación |
| :--- | :--- | :---: | :---: | :--- | :--- |
| R1 | Conteo incorrecto bloquea o permite al cliente 11 | 4 | 5 | 20 (Alto) | Particiones 0/9/10/11 y normalización |
| R2 | Listado expone clientes de otro profesional | 3 | 5 | 15 (Alto) | RLS/API con dos profesionales |
| R3 | Alta manual no funciona o duplica clientes | 4 | 4 | 16 (Alto) | Integración UI/API y unicidad |
| R4 | La acción de Plan Pro promete contratación o registra datos indebidos | 4 | 3 | 12 (Medio) | Prueba de contenido, registro de interés y ausencia de cobro |

## 2. Niveles de Prueba (Pyramid)

* **Unitarias:** Jest para normalización, conteo y transición 9/10/11.
* **Integración:** Postman/Newman para clients, appointments, RLS, alta manual y registro de interés.
* **E2E:** Playwright para listado, alta, reserva de existente/nuevo y CTA Plan Pro.

## 3. Pruebas No Funcionales

* **Seguridad:** OWASP ZAP y pruebas dirigidas de aislamiento y minimización de PII.
* **Performance:** k6 para p95 de lecturas menor o igual a 500 ms con 100 usuarios simultáneos.
* **Accesibilidad:** axe con Playwright para tabla, formulario, bloqueo y aviso permanente.

## 4. Necesidades de Entorno y Datos

| Necesidad | Entorno | ¿Disponible hoy? | Referencia |
| :--- | :--- | :--- | :--- |
| Fixtures con 0/9/10/11 clientes | Producción | No | `test-data-strategy.md` · Escenarios mínimos |
| Dos profesionales para RLS | Producción | No | `test-data-strategy.md` · Gestión de Usuarios |
| Cuenta observada con cero clientes | Producción | Parcial | `test-data-strategy.md` · Baseline observado |
| Entorno aislado con reset por `run_id` | QA requerido por Producto | No | `decisiones-po-proximo-release.md` · 9.1 y 9.2 |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Límite y CTA | `.context/PBI/epics/EPIC-CAQ-10-clientes-y-limite-freemium/epic.md` y sus Stories |
| Riesgo PII | `.context/infrastructure/test-data-strategy.md` · Privacidad y Seguridad |
| Entorno único | `.context/infrastructure/environments.md` · Mapa de Entornos |
| Reglas vigentes | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-25 a CAQ-28 |
| Scoring | **Hipótesis** — valoración de riesgo de QA |

## Contradicciones detectadas

* La carga manual está acordada, pero el botón observado no produjo ningún formulario.
* Producto limita Plan Pro a registrar interés; no se deben inferir precio, beneficios ni contratación.
* Producto exige un entorno QA aislado; el mapa vigente solo confirma Producción.

## Preguntas abiertas

* ¿Por qué «Nuevo Cliente» no abrió el formulario en la observación?
* ¿Cuándo estarán disponibles QA, fixtures 0/9/10/11, dos profesionales y teardown?
