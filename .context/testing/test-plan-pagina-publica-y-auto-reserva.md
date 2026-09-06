# Plan de Pruebas: Página pública y auto-reserva

**Epic:** CAQ-8
**Fecha:** 04/09/2026

## 1. Matriz de Riesgos del Producto

| ID | Riesgo | Probabilidad (1-5) | Impacto (1-5) | Nivel | Mitigación |
| :--- | :--- | :---: | :---: | :--- | :--- |
| R1 | Dos reservas confirmadas para el mismo slot | 4 | 5 | 20 (Alto) | Prueba concurrente y restricción transaccional |
| R2 | Slots incorrectos por tiempo o caché | 4 | 4 | 16 (Alto) | Integración de disponibilidad y límites temporales |
| R3 | Exposición de datos privados en perfil público | 3 | 5 | 15 (Alto) | Contrato API, RLS y escaneo de respuesta |
| R4 | Reintentos crean reservas duplicadas | 3 | 5 | 15 (Alto) | Idempotencia de confirmación |

## 2. Niveles de Prueba (Pyramid)

* **Unitarias:** Jest para generación de slots, validación de datos e identidad de reserva.
* **Integración:** Postman/Newman para endpoint público, RLS, revalidación e inserción concurrente.
* **E2E:** Playwright para abrir slug, elegir horario, confirmar sin cuenta y resolver conflicto.

## 3. Pruebas No Funcionales

* **Seguridad:** OWASP ZAP para exposición pública, manipulación de slug e inyección.
* **Performance:** k6 para p95 de lecturas menor o igual a 500 ms, LCP p75 menor o igual a 2,5 s y 100 usuarios simultáneos.
* **Accesibilidad:** axe con Playwright para calendario, selección, errores y actualización del conflicto.

## 4. Necesidades de Entorno y Datos

| Necesidad | Entorno | ¿Disponible hoy? | Referencia |
| :--- | :--- | :--- | :--- |
| Perfiles y slots sintéticos controlados | Producción | No | `test-data-strategy.md` · Generación de Datos Sintéticos |
| Dos sesiones concurrentes | Producción | No | `test-data-strategy.md` · Restricción inmediata |
| Consulta pública de solo lectura | Producción | Parcial | `environments.md` · Tipo de Aplicación |
| Entorno aislado para carreras de reserva | QA requerido por Producto | No | `decisiones-po-proximo-release.md` · 9.1; no figura activo en `environments.md` |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Carrera y flujo público | `.context/PBI/epics/EPIC-CAQ-8-pagina-publica-y-auto-reserva/epic.md` y sus Stories |
| Producción única | `.context/infrastructure/environments.md` · Mapa de Entornos |
| Sin datos mutantes | `.context/infrastructure/test-data-strategy.md` · Restricción inmediata |
| Reglas y objetivos vigentes | `.context/PBI/decisiones-po-proximo-release.md` · CAQ-16 a CAQ-19 y 2.6 |
| Scoring | **Hipótesis** — valoración de riesgo de QA |

## Contradicciones detectadas

* La validación no transaccional documentada no garantiza la unicidad atómica exigida por Producto.
* Producto exige un entorno QA aislado; el mapa vigente solo confirma Producción.

## Preguntas abiertas

* ¿Cuándo estarán disponibles QA, perfiles sintéticos, reset y carreras concurrentes controladas?
