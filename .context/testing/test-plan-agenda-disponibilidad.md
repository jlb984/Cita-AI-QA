# Plan de Pruebas: Agenda, disponibilidad y gestión de turnos

**Epic:** CAQ-7
**Fecha:** 04/09/2026

## 1. Matriz de Riesgos del Producto

| ID | Riesgo | Probabilidad (1-5) | Impacto (1-5) | Nivel | Mitigación |
| :--- | :--- | :---: | :---: | :--- | :--- |
| R1 | Slots erróneos por zona horaria o duración | 4 | 5 | 20 (Alto) | Unitarias de intervalos e integración API/DB |
| R2 | Bloqueo altera turnos confirmados | 3 | 5 | 15 (Alto) | Pruebas de transición y consistencia |
| R3 | Alta manual duplica un turno | 3 | 5 | 15 (Alto) | Concurrencia e idempotencia |
| R4 | Agenda expone datos de otro profesional | 3 | 5 | 15 (Alto) | RLS/API con dos cuentas |

## 2. Niveles de Prueba (Pyramid)

* **Unitarias:** Jest para intervalos, solapamientos, remanentes, duración y zonas.
* **Integración:** Postman/Newman y consultas controladas para reglas, bloqueos, appointments, RLS y slots.
* **E2E:** Playwright para configurar, bloquear, crear turno y consultar agenda.

## 3. Pruebas No Funcionales

* **Seguridad:** OWASP ZAP y pruebas de aislamiento/autorización de escrituras.
* **Performance:** k6 para p95 de lecturas menor o igual a 500 ms, 100 usuarios simultáneos y menos de 1 % de respuestas 5xx.
* **Accesibilidad:** axe con Playwright para selectores horarios, validaciones y estados vacíos.

## 4. Necesidades de Entorno y Datos

| Necesidad | Entorno | ¿Disponible hoy? | Referencia |
| :--- | :--- | :--- | :--- |
| Fixtures de bloques, duraciones y turnos | Producción | No | `test-data-strategy.md` · Generación de Datos Sintéticos |
| Dos cuentas para aislamiento | Producción | No | `test-data-strategy.md` · Gestión de Usuarios |
| Baseline de agenda vacía | Producción | Parcial | `test-data-strategy.md` · Limpieza y Reset |
| Entorno aislado con reloj y reset controlables | QA requerido por Producto | No | `decisiones-po-proximo-release.md` · 9.1; no figura activo en `environments.md` |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Riesgos de agenda | `.context/PBI/epics/EPIC-CAQ-7-agenda-disponibilidad-y-gestion-de-turnos/epic.md` y sus Stories |
| Restricción productiva | `.context/infrastructure/environments.md` · Riesgos |
| Fixtures faltantes | `.context/infrastructure/test-data-strategy.md` · Generación de Datos Sintéticos |
| Reglas y objetivos vigentes | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-12 a CAQ-15 y 2.6 |
| Scoring | **Hipótesis** — valoración de riesgo de QA |

## Contradicciones detectadas

* La decisión vigente reemplaza el entero positivo histórico por 15, 30, 45, 60, 90 y 120 minutos.
* Producto exige un entorno QA aislado; el mapa vigente solo confirma Producción.

## Preguntas abiertas

* ¿Cuándo estarán disponibles QA, reloj controlable, seed, teardown y dos cuentas sintéticas?
