---
description: Sembrar este workspace con el harness federicode (proyecto nuevo o migración de uno existente)
argument-hint: "[nombre del trabajo, opcional]"
---

Vas a **adoptar el harness federicode** en el workspace actual (la carpeta raíz de este trabajo). El formato de trabajo es global — vive en `trabajos/CLAUDE.md` y en este plugin — así que acá solo se crea **lo particular**: la anatomía de memoria y los alineadores. Los templates viven en `${CLAUDE_PLUGIN_ROOT}/templates/`.

> **Regla de oro: transformá, no destruyas.** En un workspace con trayectoria, todo lo que tiene historia (briefs, docs, decisiones, standards, mapas) es **memoria del proyecto**: se reubica o se transforma a la anatomía nueva — mecánicamente, sin reescribirlo ni reinterpretarlo. Lo **único** que se elimina es lo que duplica el *formato de trabajo* (roles/agents, commands, skills de formato, templates locales): eso lo reemplaza el plugin. Ante la duda entre particular y formato: **conservá y anotá** en el reporte.

## 1. Diagnosticá la instancia

- **A. Workspace nuevo** (sin código, sin `.claude/`): siembra pura desde templates. Pasos 2 y 4.
- **B. Workspace con trayectoria pre-harness** (código + `.claude/` en cualquier versión vieja del formato — `features/`, `roles/`, `standards/`, `ideas.md`, agents/commands locales): **transformación**. Pasos 2, 3 y 4. Nada de la historia se pierde.
- **C. Workspace parcialmente adoptado** (ya tiene la anatomía, corrida anterior incompleta): sé **idempotente** — completá solo lo que falte (fundamentals, grafo, settings), no re-siembres ni dupliques lo que ya está.

## 2. Sembrá lo que falte

- Copiá `templates/workspace/CLAUDE.md` a la raíz del workspace y completá sus particulares (qué sub-proyectos existen o se planifican, estado de cada uno). Si ya hay un CLAUDE.md viejo, **destilá sus particulares** al template nuevo — lo que era formato de trabajo queda afuera (lo aporta el plugin).
- Creá `business/` y `design-system/` desde `templates/shared/` (con su carpeta `resources/`).
- Por cada **solution** existente o justificada (web, app, backoffice, backend…), sembrá su `.claude/` desde `templates/solution/`.
- Copiá `templates/settings/settings.json` al `.claude/settings.json` de la raíz del workspace (o fusionalo si ya existe): trae los permisos del loop (edits libres, shell y commit con confirmación).
- **Topología git:** el workspace es **un solo repo** (monorepo de sub-proyectos) con raíz en la carpeta del trabajo. Si no está inicializado, `git init` en la raíz; no se crean repos por sub-proyecto.

## 3. Transformá lo viejo (solo instancia B)

| Encontrás | Lo transformás en |
|---|---|
| `features/<slug>/` (brief + tasks + doc) | `tasks/NNNN-slug/` de la misma solution — **renumeradas por cronología** (usá el git log del brief para ordenar), slug conservado, **contenido intacto**. Es un rename masivo, no una reescritura. |
| `tasks/` ya numeradas | Quedan como están — historial inmutable. |
| `ideas.md` | Pendientes → conversarlas con el Architect; parqueadas → sección *Ideas parqueadas* de `memory.md`, con motivo. |
| `standards/` (conventions, migrations…) | Convenciones vigentes → `map.md` (capa curada) de la solution; material de definición de negocio/diseño → `resources/` del shared que corresponda. |
| `map.md` / `memory.md` viejos | Se conservan y se **reencuadran** a las secciones del template nuevo — sin perder contenido. |
| Docs de diseño/plan dispersos (`plan/`, moodboards, guías, contracts) | `resources/` de `business/` o `design-system/` según corresponda, para que el Architect los procese. |
| `roles/` / `agents/` / `commands/` / `templates/` locales, y skills que **dupliquen el formato** | **Se eliminan** — el plugin los reemplaza. Skills propias del dominio del proyecto (no formato) se conservan. |

## 4. Grafo Graphify (obligatorio, uno por solution)

Cada solution con código lleva su **grafo**: es el code map automático de la tríada de memoria (grafo + `map.md` + `memory.md`).

- Prerequisito por máquina: `uv tool install graphifyy` (una vez). Si falta, frená y pedile al humano correrlo.
- Por cada solution con código: desde su raíz, `graphify update .` (construye `graphify-out/` — solo AST, sin LLM) y `graphify hook install` (reconstrucción post-commit automática).
- **No corras** `graphify claude install` ni `graphify install`: esas integraciones inyectan secciones en CLAUDE.md y hooks PreToolUse en toda sesión — acá el uso del grafo lo gobierna el harness (lo consultan Architect y Leader, no el Dev).
- Agregá `graphify-out/` al `.gitignore` del workspace — el grafo se regenera local, no se commitea.
- Solution sin código todavía: dejá anotado en su `map.md` que el grafo se crea con el primer scaffolding.

## 5. No inventes contenido

Los templates se copian con sus placeholders; **el contenido lo procesan los roles**: el Architect deriva `fundamentals/` y las definiciones de `business/`/`design-system/` desde `resources/`; el Leader completa la capa curada de `map.md` y destila `memory.md`. Tu trabajo con el material viejo es **reubicarlo**, no reescribirlo.

## 6. Reportá — y no commitees

Cerrá con un resumen: qué se creó, qué se transformó y a dónde (con el mapeo viejo→nuevo), qué se eliminó por duplicar el harness, qué quedó en duda (conservado y anotado), el estado del grafo por solution, y qué quedó pendiente para Architect (sembrar definiciones) y Leader (map/memory). **No commitees nada**: dejás el árbol de trabajo listo; el commit lo cierra el humano vía Leader.

**Workspace a adoptar:** $ARGUMENTS
