# Harness federicode

La **única fuente de verdad del formato de trabajo** de todos los trabajos. Es un marketplace local de Claude Code con un plugin (`harness`): se edita **acá, una sola vez**, y todos los proyectos que lo tienen habilitado ven la actualización — nadie mantiene copias.

## Qué es global y qué es particular

| Global (acá) | Particular (en cada workspace) |
|---|---|
| Roles: `agents/` (Architect · Dev · Leader) | `CLAUDE.md` raíz del workspace (sub-proyectos, estado, quirks) |
| Comandos: `/harness:architect` · `/harness:dev` · `/harness:leader` · `/harness:adopt` | `business/` y `design-system/` (definiciones + `resources/`) |
| Skills: `implement`, `test` | `fundamentals/`, grafo Graphify (`graphify-out/`), `map.md`, `memory.md`, `tasks/` de cada solution |
| Templates: workspace, solution, shared, task, settings | `.claude/settings.json` de cada repo (habilita el plugin + permisos) |
| El loop y las reglas → [`trabajos/CLAUDE.md`](../CLAUDE.md) (se carga solo en toda sesión bajo `trabajos/`) | El código de cada solución |

## Estructura

```
claude/
├── .claude-plugin/marketplace.json   # marketplace "federicode"
└── harness/                          # plugin "harness"
    ├── .claude-plugin/plugin.json
    ├── agents/        architect.md · dev.md · leader.md
    ├── commands/      architect.md · dev.md · leader.md · adopt.md
    ├── skills/        implement/ · test/
    └── templates/     workspace/ · solution/ · shared/ · task/ · settings/
```

## Dependencia externa: Graphify

La memoria de cada **solution** es una tríada: **grafo Graphify** (code map automático, se reconstruye post-commit) + `map.md` (capa curada) + `memory.md` (evolución histórica). Graphify es una herramienta externa instalada por máquina (`uv tool install graphifyy`). La siembra por solution la ejecuta `/harness:adopt` (fuente canónica del setup); los comandos de consulta y mantenimiento viven en el `map.md` de cada solution (sección *El grafo*).

## Topología git

Cada workspace es **un solo repo git** (monorepo de sub-proyectos): el Leader commitea en la raíz del workspace, y su `.claude/settings.json` vale para todos los sub-proyectos. El `graphify-out/` de cada solution se regenera local y va al `.gitignore` del workspace. `trabajos/` **no** se inicializa como repo (anidaría los workspaces y `claude/`); esta carpeta `claude/` lleva su propio repo.

## Modelo de sesión

Se trabaja abriendo la **raíz de cada workspace** (ej. `trabajos/dedo/`), nunca `trabajos/` — el scope de la sesión es estricto a ese workspace. `trabajos/CLAUDE.md` se carga igual (los CLAUDE.md se heredan de directorios ancestros); los roles/comandos los aporta el plugin. Para **mantener el harness**, la sesión se abre en `trabajos/` — el único contexto donde `claude/` está en scope.

## Instalación (una sola vez, a nivel usuario)

```
/plugin marketplace add c:/Users/Usuario/Documents/federicode/trabajos/claude
/plugin install harness@federicode
```

Con eso los comandos `/harness:*` y los subagentes `architect`/`dev`/`leader` quedan disponibles en **todas** las sesiones. Alternativa por-proyecto (explícita y portable): copiar `harness/templates/settings/settings.json` al `.claude/settings.json` del repo — declara el marketplace (`extraKnownMarketplaces`) y habilita el plugin (`enabledPlugins`), además de traer los permisos del loop.

## Adoptar el harness en un trabajo

Dentro del workspace (nuevo o existente): `/harness:adopt`. Siembra la anatomía desde `templates/` (o migra el formato viejo conservando lo particular: briefs, docs, standards → `resources/`), y deja pendiente para el Architect (definiciones) y el Leader (map/memory).

## Mantenimiento

- **Una mejora al formato** (roles, loop, templates, skills) se hace acá y vale para todos. Si un workspace necesita algo que el harness no da, primero preguntarse: ¿es una mejora global o un particular? Los particulares van al `CLAUDE.md`/`.claude/` del workspace, nunca como fork del harness.
- **Guardián:** el rol Leader custodia esta frontera y la config del harness — ver `agents/leader.md`, sección *Guardián del harness*.
- **Cómo propagan los cambios:** el plugin se instala **una sola vez a nivel usuario** — no hay una copia por proyecto ni versiones pinneadas por consumidor. Todos los proyectos consumen esa única instalación, que apunta a esta carpeta. Claude Code cachea el catálogo del marketplace al instalar: tras editar el harness (sobre todo cambios estructurales — renombrar comandos, mover archivos, bump de `version` en `marketplace.json`/`plugin.json`), correr **`/plugin marketplace update federicode`** refresca esa caché **una vez y para todos los proyectos a la vez**. El campo `version` es metadata del catálogo (documenta la evolución), no un lock por proyecto.
- **¿Hace falta remoto en GitHub?** No — el source es `directory` (esta carpeta local) y alcanza para una máquina. Un remoto git/GitHub solo se vuelve necesario para consumir el harness desde **otras máquinas** o compartirlo; ahí el update haría `git pull` en lugar de releer la carpeta.
- Conviene versionar esta carpeta con git (`git init` + commits): cada commit documenta la evolución del formato y habilita auto-update del marketplace.
