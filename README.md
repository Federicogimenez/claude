# Harness federicode

Un **formato de trabajo** para construir software con Claude Code — empaquetado como plugin — que reparte el trabajo entre tres roles que se equilibran, encadenados por un loop con vos y el commit como gates. Es la **única fuente de verdad del formato** de todos los trabajos: se mantiene acá, se publica en GitHub ([`Federicogimenez/claude`](https://github.com/Federicogimenez/claude)) y todos los proyectos que lo habilitan consumen la misma versión — nadie mantiene copias.

> **Orden de lectura: este README → [`harness/format.md`](harness/format.md).**
> Acá está *qué es, cómo se instala, cómo se usa y cómo se mantiene*. **El formato en sí — los roles, el loop, la anatomía del workspace, la memoria y las reglas transversales — se define una sola vez en `format.md`**, que se carga con `/harness:format`. Este README no lo reescribe: si buscás el spec, andá allá.

---

## Inicio rápido

### Una vez por máquina

Necesitás [Claude Code](https://code.claude.com/docs/en/setup) instalado y logueado (`claude` → login con tu cuenta Claude.ai). Después, en una terminal:

```
claude plugin marketplace add Federicogimenez/claude   # registra el marketplace
claude plugin install harness@federicode               # instala el plugin en tu cache de usuario
uv tool install graphifyy                              # dependencia externa: el grafo de la memoria
```

El plugin queda en `~/.claude/plugins` y sirve para **todos** tus workspaces — no se instala por proyecto. El repo es **público**: sin credenciales.

### Inicializar el harness en un proyecto

1. **Abrí la raíz del proyecto en tu IDE** (la carpeta del trabajo). La primera vez, aceptá el diálogo de confianza de Claude Code (**"Yes, I trust this folder"**) — activa los permisos del loop; sin eso, todo pide permiso.
2. **Corré `/harness:adopt`.** Un solo comando: **carga el formato completo** (equivale a `/harness:format`) y después **siembra o migra** la anatomía del workspace. Es la **única vía de siembra** — ningún rol copia templates a mano. Deja el árbol listo para que un Architect complete definiciones y un Leader cierre; no commitea.
3. **A trabajar:** `/harness:architect` arranca el loop.

> **¿Solo querés entender el formato sin inicializar nada?** Corré **`/harness:format`** — carga el spec completo sin tocar el proyecto.

---

## Por qué existe

Un ejecutor de IA es capaz de hacer casi todo, y por eso **falla de formas predecibles**: sobre-ingeniería, perder el *por qué*, desviarse del problema original, tocar cosas fuera de alcance, exigir que le re-expliques todo cada sesión. **Cada definición del formato es un contrapeso a uno de esos modos de falla.** No es burocracia: es la mínima estructura que hace repetible y coherente trabajar con IA entre muchos proyectos.

El corazón son **tres roles con contextos independientes**: el **Architect** crea (parte de la raíz del problema, investiga, diseña, arma el handoff), el **Dev** ejecuta neutral (sin opinión de diseño), el **Leader** conserva (gate de coherencia, memoria, documentación, y es el **único que commitea**). Un solo agente que crea, ejecuta y aprueba no tiene contrapeso: se auto-convence. La oposición **creador↔evaluador** con una mano neutral en el medio produce calidad sin depender de que el modelo "se porte bien" — y el humano es el único hilo que atraviesa los tres.

Hay **dos capas**: el *formato* (los conceptos → `format.md`) y la *plomería* (plugin + permisos) que hace que Claude Code lo **obligue**, no que solo lo sugiera.

**El detalle de todo eso —el loop paso a paso, la anatomía del workspace, las tres cadencias de la memoria, los aligners, las reglas transversales— está en [`harness/format.md`](harness/format.md).**

### La plomería — cómo Claude Code lo *obliga*

Esto sí es materia de este README, porque es infraestructura, no formato:

- **Plugin + marketplace** distribuyen roles, comandos, skills y templates desde GitHub; se mantiene una vez y todos consumen la misma versión (por SHA de commit).
- **`settings.json`** respalda el scope: `Edit(/**)` / `Write(/**)` acotan la escritura a la raíz del workspace, `Read(~/.claude/plugins/**)` deja leer la definición del plugin, y sendos `deny` protegen el cache y los **temporales del sistema** (para que ningún scratchpad del entorno se use en silencio). Todo con **rutas relativas, sin paths de máquina** → portable a cualquier equipo. Es respaldo, no jaula: `Bash` escribe donde quiera, así que el scope lo sostiene la norma del formato — los permisos solo encarecen equivocarse.
- **La confianza del workspace** — los `allow` solo se activan al aceptar el diálogo "Yes, I trust this folder". Nada concede capacidades sin tu consentimiento.

---

## Qué es global y qué es particular

| Global (acá, en el harness) | Particular (en cada workspace) |
|---|---|
| Roles: `agents/` (Architect · Dev · Leader) | `CLAUDE.md` raíz del workspace (sub-proyectos, estado, quirks) |
| Comandos: `/harness:architect` · `/harness:dev` · `/harness:leader` · `/harness:adopt` | `business/` y `design-system/` (definiciones + `resources/`) |
| Skills: `implement`, `test`, `format` | El norte (`fundamentals/`) y la memoria de cada solution: grafo Graphify (`graphify-out/`), `map.md`, `tasks/` |
| Templates: workspace, solution, business, design-system, task, settings | `.claude/settings.json` de cada repo (habilita el plugin + permisos) |
| El spec → [`harness/format.md`](harness/format.md), que se carga con `/harness:format` | El código de cada solution |

> **Regla de oro:** una mejora al formato se hace **acá** y vale para todos. Los particulares van al `CLAUDE.md`/`.claude/` del workspace, **nunca como fork local del harness**.

---

## Estructura del repo y cómo se carga

```
claude/
├── .claude-plugin/marketplace.json   # marketplace "federicode"
└── harness/                          # plugin "harness"
    ├── .claude-plugin/plugin.json
    ├── format.md      el spec del formato (roles, loop, anatomía, reglas)
    ├── agents/        architect.md · dev.md · leader.md
    ├── commands/      architect.md · dev.md · leader.md · adopt.md
    ├── skills/        implement/ · test/ · format/
    └── templates/     workspace/ · solution/ · business/ · design-system/ · task/ · settings/
```

Todo se entrega por el **plugin** (marketplace remoto → cache), sin clone:

- **Roles, comandos, skills y templates** — componentes estándar del plugin, con comandos namespaced `/harness:*`. Se instalan una vez por máquina.
- **`format.md`** — un plugin **no puede** inyectar un CLAUDE.md ambiental ([es explícito en la doc](https://code.claude.com/docs/en/plugins-reference): los plugins aportan contexto por skills/agents/hooks), así que el spec se carga **on-demand** con **`/harness:format`** (skill que lo lee desde el cache) o lo leen los roles desde `${CLAUDE_PLUGIN_ROOT}/format.md`. **No requiere clonar el harness.**
- **Orientación mínima por workspace** — `/harness:adopt` siembra en el `CLAUDE.md` de cada workspace las reglas duras y el puntero a `/harness:format`. Vive en el repo del workspace → siempre presente.

Instalar el plugin en cualquier máquina alcanza para que todo funcione; el checkout local solo lo necesita quien **mantiene** el harness.

---

## Consumo del plugin — detalle y reinstalación

> **Confianza del workspace:** los permisos del loop solo se activan al aceptar **"Yes, I trust this folder"**. Sin aceptarlo, cada edit y cada lectura del plugin pide permiso y parece que la sesión "ve toda la máquina". Se verifica en `~/.claude.json` → `projects[<raíz>].hasTrustDialogAccepted`.

El plugin **vive en GitHub**, no en ninguna carpeta local. Consumirlo es un flujo de dos pasos, **independiente de tener el repo clonado**:

1. **Registrás el marketplace** (`marketplace add`): Claude Code clona el repo a su estado interno (`~/.claude/plugins/marketplaces/`).
2. **Instalás el plugin** (`install`): copia el plugin al **cache** (`~/.claude/plugins/cache/<marketplace>/<plugin>/<versión>/`). Desde ahí lo cargan todas tus sesiones.

Lo que se ejecuta es la copia del cache; la fuente es el repo en GitHub. **Reinstalar desde cero** no depende de ningún archivo local:

```
claude plugin marketplace remove federicode      # quita el registro y desinstala su plugin
claude plugin marketplace add Federicogimenez/claude
claude plugin install harness@federicode
```

Verificás con `claude plugin marketplace list` o con `/plugin` dentro de una sesión.

> Borrar la carpeta local del repo **no afecta** a las sesiones que ya consumen el plugin: leen del cache. El checkout local solo importa para **mantener** el harness, o para probar cambios con `--plugin-dir`.

**Actualizar:** cada commit pusheado es una versión nueva (por SHA). `claude plugin marketplace update federicode` trae la última — o se baja sola en el auto-update en background al abrir sesiones.

**Alternativa:** si el `.claude/settings.json` del workspace ya trae `extraKnownMarketplaces` (source `github`) + `enabledPlugins` — lo que deja `/harness:adopt` —, al abrir el workspace y aceptar la confianza Claude Code te ofrece agregar el marketplace e instalar el plugin solo.

---

## Cómo se usa

Una vez adoptado, el ciclo diario:

- **`/harness:architect`** *(rol por defecto)* — entrás a diseñar. Conduce hasta el QA y **para**.
- **`/harness:dev`** (o subagente `harness:dev`) — ejecuta una skill + un handoff, sin decidir diseño. Lo invocan el Architect (implementar) y el Leader (pruebas).
- **`/harness:leader`** *(chat dedicado)* — lo disparás vos para commitear: evalúa coherencia, corre pruebas vía Dev, escribe la `doc/` de la task, mantiene `map.md` y **commitea**. Es el gate.
- **`/harness:adopt`** — siembra o migra un workspace. Única vía de siembra.
- **`/harness:format`** — carga el spec completo del formato desde el plugin.

El humano hace el **QA visual** entre el Architect y el Leader, y siempre tiene la última palabra. El paso a paso del loop, en `format.md`.

---

## Dependencia externa: Graphify

El **grafo** de la memoria por solution lo genera Graphify, una herramienta externa instalada por máquina (`uv tool install graphifyy`). La siembra por solution la ejecuta `/harness:adopt` (fuente canónica del setup); los comandos de consulta viven en el `map.md` de cada solution. Cubre **solo el código** — el `.graphifyignore` del template excluye `.claude/`.

**Mantenimiento post-commit:** el hook nativo (`graphify hook install`) asume un repo por proyecto — en el monorepo del workspace instalaría un solo hook para un solo grafo y dejaría los demás sin mantener. El harness trae un **wrapper monorepo** (`harness/templates/workspace/hooks/post-commit`) que reconstruye el grafo de cada solution tocada por el commit; `/harness:adopt` lo instala en `.git/hooks/post-commit` del workspace.

---

## Modelo de sesión y topología git

Se trabaja **abriendo la raíz de cada workspace**, nunca la carpeta que los agrupa. Para **mantener el harness**, la sesión se abre en la raíz de **este** repo, que lleva su propio git separado de los workspaces.

La topología por default es **monorepo** (un repo por workspace), con variante **poly-repo** opt-in; la decisión, sus costos y cómo se configura cada una están en `/harness:adopt` (§ *Topología git*) y en `format.md`. El `CLAUDE.md` de cada workspace registra cuál quedó.

---

## Mantenimiento

- **Esta sesión se abre con `claude --plugin-dir ./harness`.** No es una alternativa para probar cambios: es la forma canónica de abrir este repo. Carga tu working tree, así que el rol que corre es el que estás editando — sin ese flag la sesión corre la copia congelada del cache mientras vos editás otro archivo. `/reload-plugins` recarga sin reiniciar tras cada cambio.
- **Dónde vive cada definición.** El repo tiene su propia regla de gobierno — spec / persona / template, sin cuarta copia — en [`CLAUDE.md`](CLAUDE.md). Es lo que hace verificable el barrido de coherencia.
- **Guardián:** el rol Leader custodia la frontera formato↔particular y la config del harness — ver `harness/agents/leader.md`, sección *Guardián del harness*.
- **Cómo propagan los cambios: commiteás y pusheás.** El plugin instalado en otros workspaces es una copia del cache versionado, no este working tree. Cada commit pusheado es una versión nueva (por SHA). **Sin `git push`, el cambio no llega a ninguna máquina.**
- **No declares `version`** en `plugin.json` ni en la entrada del marketplace: un `version` fijo **pinea** el plugin y los commits nuevos dejan de propagarse sin aviso. Sin el campo, la versión cae al SHA del commit. (La doc además desaconseja declararlo en los dos lados: el de `plugin.json` gana en silencio.)

---

## Hacer un fork (tu propio harness)

Si querés partir de este harness para armar el tuyo:

1. **Cloná o forkeá** el repo a tu propio GitHub (`tu-usuario/tu-repo`).
2. **`.claude-plugin/marketplace.json`** — cambiá `name` (el nombre del marketplace, hoy `federicode`) y `owner`. Ese `name` es con el que se instala: `<plugin>@<name>`.
3. **`harness/.claude-plugin/plugin.json`** — actualizá `author`. El `name` del plugin (`harness`) podés dejarlo o cambiarlo; si lo cambiás, cambia el namespace de los comandos (`/tu-plugin:*`) **y el `subagent_type` de los roles** (`harness:dev` → `tu-plugin:dev`), que las personas citan explícitamente.
4. **`harness/templates/settings/settings.json`** — apuntá `extraKnownMarketplaces.<tu-marketplace>.source.repo` a `tu-usuario/tu-repo`, y `enabledPlugins` a `<plugin>@<tu-marketplace>`. (Los permisos `Edit(/**)` / `Write(/**)` / `Read(~/.claude/plugins/**)` y los `deny` de cache y temporales son portables — no los toques.)
5. **Publicá** con `git push` y consumí como en *Inicio rápido*, con tu repo y tu marketplace.

Revisá `format.md` y los `agents/` por menciones de marca ("federicode") si querés renombrar. No declares `version` — dejá que el SHA versione.
