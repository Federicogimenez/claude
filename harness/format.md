# federicode — Formato de trabajo global

Este archivo se carga en **toda sesión bajo `trabajos/`** — lo importa el stub `trabajos/CLAUDE.md`. Define el formato de trabajo común a todos los trabajos; manténgase corto — cada token aquí se paga siempre. **Lo global vive acá, en el harness (`trabajos/claude/`, plugin `harness@federicode`); lo particular vive en cada workspace.** Una mejora al formato se hace en el harness, nunca en una copia local.

## Anatomía de un trabajo (workspace)

Cada carpeta de primer nivel en `trabajos/` es un **workspace**: un monorepo de sub-proyectos interdependientes con su `CLAUDE.md` raíz (solo particulares). Dos shared lo siembran y alinean, y las soluciones de software lo materializan:

- **`business/`** — modelo de negocio; su `.claude/CLAUDE.md` es la **autoridad de integración** (qué sub-proyectos existen, qué expone/consume cada uno, qué decisiones de negocio condicionan el diseño).
- **`design-system/`** — el **lenguaje visual** que alinea todo; toda task de diseño se ancla acá.
- **Solutions** (`web/`, `app/`, `backoffice/`, `backend/`…): skeleton completo `fundamentals` + tríada de memoria + `tasks`. Se arranca con las que `business/` justifique.

`business/` y `design-system/` se siembran desde su carpeta **`resources/`** (material crudo que carga el humano; el Architect lo procesa en definiciones, el Leader las mantiene veraces).

> **Workspace sin esta anatomía** (proyecto viejo o nuevo): no improvises un formato local — corré `/harness:adopt` para sembrarlo o migrarlo.

## Los roles

Dos enfoques **opuestos** balancean el trabajo — uno **crea**, el otro **evalúa** — y una mano neutral **ejecuta** entre ambos. Las personas viven en el plugin (`harness@federicode`, carpeta `agents/`), no en cada repo.

| Rol | Carácter | Hace | Entrada |
|---|---|---|---|
| **Architect** | Creativo, filósofo de la raíz | Procesa resources, investiga, diseña, pesa la task, arma el handoff del Dev | `/harness:architect` (rol por defecto) |
| **Dev** | Ejecutor neutral, sin opinión de diseño | Toma una skill (enfoque) + handoff y ejecuta dentro del scope | `/harness:dev` (o como subagente `dev`) |
| **Leader** | Conservador, guardián | Gate de coherencia, mantiene map/memory/integración, **único que commitea** | `/harness:leader` en chat dedicado |

Architect y Leader mantienen contexto y decisiones independientes: por eso se equilibran. El Dev no decide diseño — sirve a ambos.

## El loop

1. **Vos ↔ Architect** entienden la raíz → investiga → diseña. Pesa la task (**liviana/completa**) y arma el **handoff** autosuficiente (instrucción condensada + scope de archivos + punteros — el Dev no lee el `map.md`).
2. **Dev** ejecuta sobre la skill + el handoff, con contexto solo de ejecución, todos los edits de corrido.
3. **Vos hacés QA visual.** El Architect surfacea el resultado y **para acá** — el Leader no se encadena.
4. Ajustes del QA → **corrección por delta al mismo Dev** (sin reboot) → volvés a QA, hasta tu OK.
5. **Vos disparás al Leader** cuando decidís commitear: gate de coherencia proporcional al peso + pruebas (vía Dev, skill `test`) + commit + actualiza `map.md` y `memory.md`.

**Gate obligatorio:** ningún `git commit` sin el Leader. Vos siempre tenés la última palabra — el Leader propone, no impone.

## Reglas transversales

- **Scope de sesión estricto:** se trabaja abriendo la **raíz del workspace** (carpeta hermana de `claude/`, ej. `trabajos/dedo/`), nunca `trabajos/`. Todo lo que la sesión toca vive dentro de ese workspace — nada fuera, y el `settings.json` del workspace lo hace cumplir (`Edit` permitido solo bajo su raíz, `claude/` denegado). El harness — **este archivo incluido** — se edita solo en sesiones abiertas en `trabajos/claude/`, su propia raíz.
- **Sesgo a la simplicidad:** resolver simple, no robusto. Sobre-ingeniería es deuda; robustez solo cuando es muy necesaria y justificada.
- **Cross-project:** un cambio que toca varios sub-proyectos = una task por proyecto, con vínculo declarado en cada `brief.md`; la integración de fondo queda en `business/.claude/CLAUDE.md`.
- **Toda solución parte de un problema:** si una task no rastrea a `fundamentals/` (o al norte de `business/`), no se construye.
- **Tríada de memoria por solution:** **grafo Graphify** (code map automático — estructura y conexiones, se reconstruye post-commit; se consulta con `graphify query|explain|path`, uno por solution) + `map.md` (capa curada: infra, integración, convenciones — solo lo que el grafo no infiere) + `memory.md` (evolución histórica: decisiones, principios, deuda, ideas parqueadas con motivo). Más `tasks/NNNN-slug/` (historial inmutable: `brief.md` + `tasks.md` + `doc/`). El Dev no consulta map ni grafo — recibe punteros destilados en el handoff.
- **Skills:** internas (`implement`, `test` — solo el enfoque; la disciplina vive en la persona del Dev), instaladas (marketplace de terceros) y bundled (ej. `claude-api`, fuente autoritativa de modelos — se invoca, no se copia).
- **Templates:** workspace, solution, shared, task y settings viven en el harness (`trabajos/claude/harness/templates/`). Task nueva = copiar `templates/task/` y numerar secuencial (`0001-`, `0002-`, …).
