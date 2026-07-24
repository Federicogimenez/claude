# Harness federicode

La **única fuente de verdad del formato de trabajo** de todos los trabajos. Es un marketplace de Claude Code **hospedado en GitHub** ([`Federicogimenez/claude`](https://github.com/Federicogimenez/claude)) con un plugin (`harness`): se mantiene **acá, una sola vez**, se publica con `git push`, y todos los proyectos que lo tienen habilitado consumen la misma versión publicada — nadie mantiene copias.

## Qué es global y qué es particular

| Global (acá) | Particular (en cada workspace) |
|---|---|
| Roles: `agents/` (Architect · Dev · Leader) | `CLAUDE.md` raíz del workspace (sub-proyectos, estado, quirks) |
| Comandos: `/harness:architect` · `/harness:dev` · `/harness:leader` · `/harness:adopt` | `business/` y `design-system/` (definiciones + `resources/`) |
| Skills: `implement`, `test` | `fundamentals/`, grafo Graphify (`graphify-out/`), `map.md` (capa curada + *Evolución*), `tasks/` de cada solution |
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

La memoria de cada **solution** son dos capas + la crónica inmutable: **grafo Graphify** (code map automático, se reconstruye post-commit) + `map.md` (capa curada: infra, integración, convenciones, y al final una sección *Evolución* con la síntesis vigente — principios, deuda, ideas parqueadas) — más `tasks/` (historial inmutable: la crónica del *por qué* por cambio). Graphify es una herramienta externa instalada por máquina (`uv tool install graphifyy`). La siembra por solution la ejecuta `/harness:adopt` (fuente canónica del setup); los comandos de consulta y mantenimiento viven en el `map.md` de cada solution (sección *El grafo*). El grafo cubre **solo el código** de la solution — el `.graphifyignore` del template excluye `.claude/`: es el sobrevuelo optimizado del code base; el detalle y el porqué viven en el `map.md` y en `tasks/`.

**Mantenimiento post-commit:** el hook nativo (`graphify hook install`) asume un repo por proyecto — en el monorepo del workspace instalaría un solo hook para un solo grafo y dejaría los demás sin mantenimiento. El harness trae un **wrapper monorepo** (`harness/templates/workspace/hooks/post-commit`) que reconstruye el grafo de cada solution tocada por el commit; `/harness:adopt` lo instala en `.git/hooks/post-commit` del workspace.

## Topología git

Cada workspace es **un solo repo git** (monorepo de sub-proyectos): el Leader commitea en la raíz del workspace, y su `.claude/settings.json` vale para todos los sub-proyectos. El `graphify-out/` de cada solution se regenera local y va al `.gitignore` del workspace. `trabajos/` **no** se inicializa como repo (anidaría los workspaces y `claude/`); esta carpeta `claude/` lleva su propio repo.

## Modelo de sesión

Se trabaja abriendo la **raíz de cada workspace** (ej. `trabajos/dedo/`), nunca `trabajos/` — el scope de la sesión es estricto a ese workspace. `trabajos/CLAUDE.md` se carga igual (los CLAUDE.md se heredan de directorios ancestros) y arrastra por import a `harness/format.md`; los roles/comandos los aporta el plugin. Para **mantener el harness**, la sesión se abre en **`trabajos/claude/`** — su propia raíz, hermana de los workspaces y no su padre.

El scope no es solo prosa: el `settings.json` que `/harness:adopt` deja en cada workspace permite editar únicamente bajo su raíz (`Edit(/**)`, que ancla en la raíz del workspace) y **deniega** editar el cache del plugin (`Edit(~/.claude/plugins/**)`). Un `deny` gana sobre cualquier `allow`, así que una sesión de workspace no puede tocar el harness instalado ni por accidente. Todo con rutas relativas (`/…` al workspace, `~/…` al home): **ni un path absoluto de máquina**, para que el mismo settings sirva en cualquier equipo.

## Máquina nueva — paso a paso (primera vez con Claude Code)

Para alguien que arranca de cero, sin nada instalado:

**1. Instalá Claude Code y logueate.** Instalá la CLI o la extensión de VSCode/JetBrains (ver la [doc oficial de instalación](https://code.claude.com/docs/en/setup)). Abrí una terminal y corré `claude` (o abrí la extensión): la primera vez abre el navegador para loguearte con tu cuenta de Claude.ai (Pro/Max/Team). Cuando ves `Login successful`, listo.

**2. Instalá el plugin del harness — una vez por máquina.** En una terminal:

```
claude plugin marketplace add Federicogimenez/claude
claude plugin install harness@federicode
```

El plugin queda en el **cache de usuario** (`~/.claude/plugins`) y vale para **todos** tus workspaces — no se instala por proyecto. Con eso los comandos `/harness:*` y los subagentes `architect`/`dev`/`leader` quedan disponibles en toda sesión.

> El repo es **público**: no hace falta ninguna credencial. (Si algún día pasa a privado, configurás git auth una vez con `gh auth setup-git`.)

**3. Instalá Graphify — una vez por máquina.** La memoria de código de cada solution usa Graphify (ver más abajo): `uv tool install graphifyy`. Necesitás [`uv`](https://docs.astral.sh/uv/) instalado.

**4. Abrí un workspace y aceptá la confianza.** Abrí la **raíz del workspace** donde vas a trabajar. La primera vez, Claude Code muestra el diálogo de confianza: aceptá **"Yes, I trust this folder"**. Recién ahí se activan los permisos del loop (edición libre en el scope, lectura del plugin); **sin aceptarlo, cada edit y cada lectura del plugin pide permiso** y parece que la sesión "ve toda la máquina". Si el workspace todavía no tiene la anatomía del harness, corré `/harness:adopt`.

**5. A trabajar.** `/harness:architect` arranca el loop.

**Actualizar el harness después:** cada commit al repo del harness es una versión nueva (por SHA). Traés la última con `claude plugin marketplace update federicode` en la terminal — o dejás que Claude Code la baje solo en el auto-update en background al abrir sesiones.

## Cómo se consume el plugin — y reinstalar desde cero

El plugin **vive en GitHub**, no en ninguna carpeta local. Consumirlo es un flujo de dos pasos, **independiente de tener el repo clonado en tu disco**:

1. **Registrás el marketplace** — `claude plugin marketplace add Federicogimenez/claude`: Claude Code clona el repo a su estado interno de marketplaces (`~/.claude/plugins/marketplaces/`).
2. **Instalás el plugin** — `claude plugin install harness@federicode`: copia el plugin al **cache de usuario** (`~/.claude/plugins/cache/<marketplace>/<plugin>/<versión>/`). Desde ahí lo cargan **todas** tus sesiones.

Lo que se ejecuta es la copia del cache; la fuente es el repo en GitHub. No necesitás un checkout local para usar el plugin: el `add` lo trae solo desde el remoto.

**Reinstalar desde cero** — borraste un checkout local, limpiaste el cache, o algo quedó inconsistente. No depende de ningún archivo local; se reconstruye todo desde GitHub:

```
claude plugin marketplace remove federicode      # quita el registro y desinstala su plugin (si estaba)
claude plugin marketplace add Federicogimenez/claude
claude plugin install harness@federicode
```

El `add` vuelve a clonar desde GitHub y el `install` reconstruye el cache. Para verificar el estado: `claude plugin marketplace list` (marketplaces registrados) o `/plugin` dentro de una sesión (plugins instalados).

> **Borrar solo la carpeta local del repo no afecta a las sesiones que ya consumen el plugin:** ellas leen del cache, no de tu checkout. El checkout local solo importa para **mantener** el harness (editar y publicar con `git push`) o para probar cambios sin publicar (`claude --plugin-dir <ruta-al-plugin>`).

### Alternativa: que el workspace lo pida solo

Si el `.claude/settings.json` del workspace ya trae `extraKnownMarketplaces` (source `github`) + `enabledPlugins` — lo que deja `/harness:adopt` — podés saltarte el **paso 2** manual: al abrir el workspace y aceptar la confianza, Claude Code te ofrece agregar el marketplace e instalar el plugin. Aceptás y queda igual en el cache de usuario, disponible para todos tus workspaces.

## Adoptar el harness en un trabajo

Dentro del workspace (nuevo o existente): `/harness:adopt`. Siembra la anatomía desde `templates/` (o migra el formato viejo conservando lo particular: briefs, docs, standards → `resources/`), y deja pendiente para el Architect (definiciones) y el Leader (map/memory).

## Mantenimiento

- **Una mejora al formato** (roles, loop, templates, skills) se hace acá y vale para todos. Si un workspace necesita algo que el harness no da, primero preguntarse: ¿es una mejora global o un particular? Los particulares van al `CLAUDE.md`/`.claude/` del workspace, nunca como fork del harness.
- **Guardián:** el rol Leader custodia esta frontera y la config del harness — ver `agents/leader.md`, sección *Guardián del harness*.
- **Cómo propagan los cambios (consumidores): commiteás y pusheás.** El harness se sirve desde GitHub, así que el plugin instalado es una **copia del cache versionado** (`~/.claude/plugins/cache/…`), no el working tree vivo. Cada commit pusheado es una versión nueva (por SHA); los workspaces la reciben con `claude plugin marketplace update federicode` o por el auto-update en background al arrancar sesión. **Sin `git push`, el cambio no llega a ninguna máquina.**
- **Probar cambios sin publicar (mantenedor):** abrí el harness con `claude --plugin-dir ./harness` — carga tu working tree y pisa la versión instalada **solo en esa sesión**, sin commit ni push. Es el reemplazo del viejo modelo "editás y reiniciás" de cuando el source era `directory` local. `/reload-plugins` recarga sin reiniciar.
- **El commit + push es el gate.** El Leader es el único que commitea; el push publica. Dejar el working tree sucio no propaga nada y pierde la trazabilidad del formato — que es todo lo que este repo aporta.
- **No declares `version`** en `plugin.json` ni en la entrada del marketplace. Con la caché versionada del install remoto, un `version` fijo **pinea** el plugin: los commits nuevos dejan de propagarse sin que nada avise. Sin el campo, la versión cae al SHA del commit y cada commit es una versión nueva. La doc además desaconseja declararlo en los dos lados, porque el de `plugin.json` gana en silencio.
