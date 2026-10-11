# Iniciativa Impulso IA — Repositorio de trabajo de QA

Este repositorio no contiene la aplicación ni el código del producto. Es el espacio de trabajo del equipo de QA para la **Iniciativa Impulso IA**, que implementa el uso de Inteligencia Artificial en un proyecto de desarrollo.

Su propósito es centralizar el contexto, la documentación técnica, los artefactos de análisis, la trazabilidad de calidad y los materiales que apoyan la ejecución del equipo con IA como asistente técnico.

---

## ¿Qué es este repositorio?

Este repositorio funciona como la memoria operativa del proyecto de QA. Aquí se almacenan:

- documentación del negocio y del contexto de la solución,
- definiciones funcionales y técnicas,
- entregables de análisis, revisión y validación,
- evidencia de pruebas y observaciones,
- guías de uso de herramientas del entorno,
- prompts y flujos de trabajo para apoyar la ejecución del equipo.

En otras palabras, este repositorio sirve para trabajar el contexto del proyecto, no para alojar la aplicación en sí.

---

## Estructura de carpetas

| Carpeta | Para qué sirve |
| :--- | :--- |
| [`.prompts/`](.prompts/) | Contiene los flujos de trabajo, prompts y plantillas por fase. Guían análisis, especificación, revisión y documentación del proyecto. |
| [`.context/`](.context/) | Es la base de conocimiento del proyecto. Aquí se guardan documentos, evidencias, decisiones y entregables que alimentan el trabajo del equipo y de la IA. |
| [`.documents/`](.documents/) | Guarda la base teórica y metodológica, con la explicación del enfoque por fases y el marco de trabajo. |
| [`.guides/`](.guides/) | Tiene guías técnicas del entorno: Git, LLMs, MCPs, Postman y setup inicial. |
| [`.agents/`](.agents/) | Acumula skills y procedimientos reutilizables para asistentes. |
| [`.claude/`](.claude/) | Contiene la configuración y enlaces específicos para Claude Code y las skills vinculadas. |
| [`.github/`](.github/) | Configuración y automatizaciones del repositorio en GitHub. |
| [AGENTS.md](AGENTS.md) | Es la fuente única de directivas y convenciones del repositorio. |
| [CLAUDE.md](CLAUDE.md) | Puntero de trabajo y referencia a las reglas del repositorio. |

### Qué contiene `.context/`

La carpeta [`.context/`](.context/) es el núcleo documental del proyecto. Allí se guarda el contexto operativo que alimenta la actividad del equipo y la IA.

Incluye, entre otras áreas:

- [`.context/Confluence-corporativo/`](.context/Confluence-corporativo/) : registro histórico documental del proyecto, conservado como evidencia de la etapa inicial.
- [`.context/idea/`](.context/idea/) : contexto de constitución, definición y análisis inicial.
- [`.context/architecture/`](.context/architecture/) : decisiones y documentación de arquitectura.
- [`.context/infrastructure/`](.context/infrastructure/) : infraestructura y entorno técnico.
- [`.context/PBI/`](.context/PBI/) : especificaciones, backlog y trazabilidad funcional.
- [`.context/testing/`](.context/testing/) : artefactos de testing y validación.
- [`.context/project-connections.md`](.context/project-connections.md) : referencias y conexiones del proyecto.
- [`.context/ai-project-map.md`](.context/ai-project-map.md) : mapa general del proyecto y su estructura de trabajo.

---

## Guías del entorno

Las siguientes guías siguen siendo válidas y forman parte de la base operativa del repositorio:

- [Inicio y setup](.guides/guia-inicio-setup.md)
- [Git — básico](.guides/Git/git-basico.md)
- [Git — flujo de trabajo](.guides/Git/git-workflow.md)
- [Git — trabajo en equipo](.guides/Git/git-colaboracion.md)
- [LLMs](.guides/LLMs/)
- [MCPs](.guides/mcps/)
- [Postman](.guides/postman/)

---

## Cómo se trabaja en este repositorio

El flujo de trabajo se apoya en:

1. leer y comprender el contexto del proyecto antes de producir nuevos artefactos,
2. usar los prompts y plantillas de [`.prompts/`](.prompts/) para guiar análisis y entregas,
3. conservar la evidencia y los resultados en [`.context/`](.context/),
4. aplicar metodologías de QA y validación con el contexto técnico disponible,
5. usar las guías de [`.guides/`](.guides/) para preparar el entorno, trabajar con Git y conectar herramientas.

---

## Objetivo del repositorio

El objetivo de este repositorio es apoyar la operación de calidad del proyecto, ayudando al equipo a:

- comprender el contexto del producto y de la solución,
- documentar y revisar requisitos,
- analizar riesgos y cobertura,
- validar comportamiento y evidencia,
- trabajar con IA como complemento del análisis y la calidad,
- mantener trazabilidad y consistencia técnica del trabajo.

Es el repositorio operativo del trabajo de QA para la **Iniciativa Impulso IA**.