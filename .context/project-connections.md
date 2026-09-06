# Conexiones oficiales del proyecto

Este archivo define los destinos canónicos de esta ejecución. Toda guía, prompt y
entregable operativo debe resolver las conexiones desde aquí o desde `AGENTS.md`.

| Servicio | Valor oficial |
| :--- | :--- |
| Jira | `https://jlb984.atlassian.net/` |
| Project Key de Jira | `CAQ` |
| GitHub | `https://github.com/jlb984/Cita-AI-QA` |

## Regla de uso

* `CAQ` es el único proyecto Jira autorizado para nuevas lecturas, escrituras y
  sincronizaciones de este repositorio.
* El repositorio GitHub oficial es `jlb984/Cita-AI-QA`; no se debe publicar
  esta ejecución en repositorios anteriores del curso.
* Las claves `BJHB-*` proceden de otra ejecución sobre la misma aplicación. El
  06/09/2026 se compararon, deduplicaron y adaptaron a los tickets reales de `CAQ`; el
  mapeo auditable está en `.context/PBI/reconciliacion-backlog-caq.md`.
* Las referencias operativas locales usan las claves reales de `CAQ`. Las claves
  `BJHB-*` solo deben aparecer como trazabilidad histórica de la importación.
* Ningún flujo debe leer o modificar `BJHB` como consecuencia de trabajar en este
  repositorio.

## Precedencia

Si otro documento editable contradice estos valores, prevalece este archivo. Los
archivos de `.context/Confluence-corporativo/` son un registro histórico inmutable y
pueden conservar nombres o referencias anteriores sin que eso cambie los destinos
oficiales.
