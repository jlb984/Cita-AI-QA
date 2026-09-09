# Plan de Pruebas: Cancelaciones y comunicaciones transaccionales

**Epic:** CAQ-9
**Fecha:** 04/09/2026

## 1. Matriz de Riesgos del Producto

| ID | Riesgo | Probabilidad (1-5) | Impacto (1-5) | Nivel | Mitigación |
| :--- | :--- | :---: | :---: | :--- | :--- |
| R1 | Cancelación aparenta éxito sin persistir | 5 | 5 | 25 (Alto) | E2E con verificación API/DB y recarga |
| R2 | Slot cancelado no vuelve a ofrecerse | 5 | 4 | 20 (Alto) | Integración cancelación-disponibilidad |
| R3 | Token permite cancelar turno indebido | 3 | 5 | 15 (Alto) | Seguridad de token y autorización |
| R4 | Correos ausentes, duplicados o incorrectos | 4 | 4 | 16 (Alto) | Contratos, buzón controlado e idempotencia |
| R5 | Scheduler omite o duplica recordatorios de 24 horas | 4 | 4 | 16 (Alto) | Reloj controlado, estados y deduplicación |

## 2. Niveles de Prueba (Pyramid)

* **Unitarias:** Jest para transiciones, ventanas, tokens, destinatarios y deduplicación.
* **Integración:** Postman/Newman para appointments, disponibilidad, Resend y Supabase Auth.
* **E2E:** Playwright para ambas cancelaciones, recarga, slot liberado y correos.

## 3. Pruebas No Funcionales

* **Seguridad:** OWASP ZAP y pruebas dirigidas de entropía/vigencia de token, aislamiento y enumeración.
* **Resiliencia:** Postman/Newman para fallos y reintentos de Resend sin estados engañosos.
* **Performance:** k6 para p95 de lecturas menor o igual a 500 ms, 100 usuarios simultáneos y menos de 1 % de respuestas 5xx.

## 4. Necesidades de Entorno y Datos

| Necesidad | Entorno | ¿Disponible hoy? | Referencia |
| :--- | :--- | :--- | :--- |
| Turnos futuros/pasados y buzones interceptados | Producción | No | `test-data-strategy.md` · Escenarios mínimos |
| Acceso de lectura a turno sintético existente | Producción | Parcial | `environments.md` · Detalles de Acceso |
| Control del reloj y scheduler | Producción | No | `test-data-strategy.md` · Capacidad faltante |
| Entorno aislado con correo sandbox | QA requerido por Producto | No | `decisiones-po-proximo-release.md` · 9.1 y 9.2 |

## Fuentes

| Dato / afirmación | De dónde sale |
| :--- | :--- |
| Falla observada de CAQ-21 | `.context/PBI/epics/EPIC-CAQ-9-cancelaciones-y-comunicaciones-transaccionales/stories/STORY-CAQ-21-cancelacion-por-profesional/story.md` |
| Restricción de entorno | `.context/infrastructure/environments.md` · Riesgos |
| Datos necesarios | `.context/infrastructure/test-data-strategy.md` · Escenarios mínimos |
| Reglas vigentes | `.context/product-decisions/decisiones-po-proximo-release.md` · CAQ-20 a CAQ-23 y 7.1 |
| Scoring | **Hipótesis** — valoración de riesgo de QA |

## Contradicciones detectadas

* CAQ-21 exige persistencia y liberación antes del éxito; producción mostró el turno `confirmed` y el slot ocupado.
* Producto reincorpora el recordatorio al release 1.1, reemplazando su exclusión del primer lanzamiento.
* Producto exige un entorno QA aislado; el mapa vigente solo confirma Producción.

## Preguntas abiertas

* ¿Cuál fue la causa técnica del falso éxito de CAQ-21 y se intentó enviar el correo?
* ¿Cuándo estarán disponibles QA, correo sandbox, reloj controlable, seed y teardown?
