# Harness federicode

La **única fuente de verdad del formato de trabajo** de todos los trabajos. Es un marketplace local de Claude Code con un plugin (`harness`): se edita **acá, una sola vez**, y todos los proyectos que lo tienen habilitado ven la actualización — nadie mantiene copias.

## Qué es global y qué es particular

| Global (acá) | Particular (en cada workspace) |
|---|---|
| Roles: `agents/` (Architect · Dev · Leader) | `CLAUDE.md` raíz del workspace (sub-proyectos, estado, quirks) |
| Comandos: `/harness:architect` · `/harness:dev` · `/harness:leader` · `/harness:adopt` | `business/` y `design-system/` (definiciones + `resources/`) |
| Skills: `implement`, `test` | `fundamentals/`, grafo Graphify (`graphify-out/`), `map.md`, `memory.md`, `tasks/` de cada solution |
| Templates: workspace, solution, shared, task, settings | `.claude/settings.json` de cada repo (habilita el plugin + permisos) |
| El loop y las reglas → [`harness/format.md`](harness/format.md) (lo importa el stub `trabajos/CLAUDE.md`, y así se carga en toda sesión bajo `trabajos/`) | El código de cada solución |

## Estructura

```
claude/
├── .claude-plugin/marketplace.json   # marketplace "federicode"
└── harness/                          # plugin "harness"
    ├── .claude-plugin/plugin.json
    ├── format.md      el formato de trabajo (loop, roles, reglas)
    ├── agents/        architect.md · dev.md · leader.md
    ├── commands/      architect.md · dev.md · leader.md · adopt.md
    ├── skills/        implement/ · test/
    └── templates/     workspace/ · solution/ · shared/ · task/ · settings/
```

`format.md` no lo carga el plugin: lo importa el stub `trabajos/CLAUDE.md` por ruta (`@claude/harness/format.md`). Por eso vive en `harness/` pero **no** es un directorio del plugin — es el doc del formato, versionado junto a los roles que lo implementan.

## Dependencia externa: Graphify

La memoria de cada **solution** es una tríada: **grafo Graphify** (code map automático, se reconstruye post-commit) + `map.md` (capa curada) + `memory.md` (evolución histórica). Graphify es una herramienta externa instalada por máquina (`uv tool install graphifyy`). La siembra por solution la ejecuta `/harness:adopt` (fuente canónica del setup); los comandos de consulta y mantenimiento viven en el `map.md` de cada solution (sección *El grafo*). El grafo cubre **solo el código** de la solution — el `.graphifyignore` del template excluye `.claude/`: es el sobrevuelo optimizado del code base; el detalle vive en la memoria del proyecto.

**Mantenimiento post-commit:** el hook nativo (`graphify hook install`) asume un repo por proyecto — en el monorepo del workspace instalaría un solo hook para un solo grafo y dejaría los demás sin mantenimiento. El harness trae un **wrapper monorepo** (`harness/templates/workspace/hooks/post-commit`) que reconstruye el grafo de cada solution tocada por el commit; `/harness:adopt` lo instala en `.git/hooks/post-commit` del workspace.

## Topología git

Cada workspace es **un solo repo git** (monorepo de sub-proyectos): el Leader commitea en la raíz del workspace, y su `.claude/settings.json` vale para todos los sub-proyectos. El `graphify-out/` de cada solution se regenera local y va al `.gitignore` del workspace. `trabajos/` **no** se inicializa como repo (anidaría los workspaces y `claude/`); esta carpeta `claude/` lleva su propio repo.

## Modelo de sesión

Se trabaja abriendo la **raíz de cada workspace** (ej. `trabajos/dedo/`), nunca `trabajos/` — el scope de la sesión es estricto a ese workspace. `trabajos/CLAUDE.md` se carga igual (los CLAUDE.md se heredan de directorios ancestros) y arrastra por import a `harness/format.md`; los roles/comandos los aporta el plugin. Para **mantener el harness**, la sesión se abre en **`trabajos/claude/`** — su propia raíz, hermana de los workspaces y no su padre.

El scope no es solo prosa: el `settings.json` que `/harness:adopt` deja en cada workspace permite editar únicamente bajo su raíz (`Edit(/**)`) y **deniega** explícitamente `trabajos/claude/**`. Un `deny` gana sobre cualquier `allow`, así que una sesión de workspace no puede tocar el harness ni por accidente ni por insistencia.

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
- **Cómo propagan los cambios: editás y reiniciás. Nada más.** Con source `directory`, Claude Code registra el marketplace **en esta carpeta** (`installLocation`) y lee agents, commands, skills y templates de acá, en vivo del working tree. No hay copia intermedia: `~/.claude/plugins/cache/` no existe. Ni `/plugin update`, ni bump de versión, ni commit — el cambio está activo en la próxima sesión que arranque. *(Verificado el 2026-07-22 con un marcador sin commitear en la `description` de una skill: apareció en el system prompt tras reiniciar.)*
  > El `installPath` bajo `cache/` que figura en `~/.claude/plugins/installed_plugins.json` es contabilidad del install original y apunta a una ruta que no existe. No lo persigas.
- **El commit es el gate, no el mecanismo.** Como la propagación no lo necesita, es fácil dejar el working tree sucio y perder la trazabilidad del formato — que es todo lo que este repo aporta. Commiteá igual, con el Leader.
- **No declares `version`** en `plugin.json` ni en la entrada del marketplace. Hoy no cambia nada (no hay caché que versionar), pero el día que este harness se sirva desde un remoto sí aparece la caché versionada, y un `version` fijo **pinea** el plugin: los commits nuevos dejan de propagarse sin que nada avise. Sin el campo, la versión cae al SHA del commit y cada commit es una versión nueva. La doc además desaconseja declararlo en los dos lados, porque el de `plugin.json` gana en silencio.
- **¿Hace falta remoto en GitHub?** No — el source `directory` alcanza para una máquina, y encima es lo que da la lectura en vivo. Un remoto solo se vuelve necesario para consumir el harness desde **otras máquinas**; ahí sí entra la caché versionada, el `git pull` y el `/plugin update`.
