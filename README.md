# Harness federicode

Un **formato de trabajo** para construir software con Claude Code — empaquetado como plugin — que reparte el trabajo entre tres roles que se equilibran, encadenados por un loop con vos y el commit como gates. Es la **única fuente de verdad del formato** de todos los trabajos: se mantiene acá, se publica en GitHub ([`Federicogimenez/claude`](https://github.com/Federicogimenez/claude)) y todos los proyectos que lo habilitan consumen la misma versión — nadie mantiene copias.

---

## Qué es y qué resuelve

Un ejecutor de IA es capaz de hacer casi todo, y por eso **falla de formas predecibles**: sobre-ingeniería, perder el *por qué*, desviarse del problema original, tocar cosas fuera de alcance, exigir que le re-expliques todo cada sesión. **Cada definición del formato es un contrapeso a uno de esos modos de falla.** No es burocracia: es la mínima estructura que hace repetible y coherente trabajar con IA entre muchos proyectos.

Hay **dos capas**: el *formato* (los conceptos, acá abajo) y la *plomería* (plugin + permisos) que hace que Claude Code lo **obligue**, no que solo lo sugiera.

### Los tres roles — el corazón

Las personas viven en `agents/`; se invocan por comando (`/harness:<rol>`) o como subagente. **Architect y Leader mantienen contextos y decisiones independientes** — son dos cabezas separadas a propósito.

| Rol | Fuerza | Qué evita |
|---|---|---|
| **Architect** | Crea: parte de la raíz del problema, investiga, diseña, arma el handoff | Construir sin entender el problema |
| **Dev** | Ejecuta, **neutral, sin opinión de diseño** | Que quien diseña también ejecute y racionalice sus propios errores |
| **Leader** | Conserva: gate de coherencia, **único que commitea** | La deriva silenciosa entre lo que se quería y lo construido |

Un solo agente que crea, ejecuta y aprueba no tiene contrapeso: se auto-convence. La oposición **creador↔evaluador** con una mano neutral en el medio produce calidad sin depender de que el modelo "se porte bien".

### El loop

1. **Vos ↔ Architect** entienden la raíz, diseñan, pesan la task (liviana/completa) y arman el **handoff** autosuficiente (instrucción condensada + scope de archivos + punteros).
2. **Dev** ejecuta solo eso, con contexto solo de ejecución, todos los edits de corrido.
3. **Vos hacés QA visual.** El Architect surfacea el resultado y **para** — el Leader no se encadena.
4. Ajustes del QA → **corrección por delta al mismo Dev** (sin reboot) → volvés a QA, hasta tu OK.
5. **Vos disparás al Leader**: gate de coherencia proporcional al peso + pruebas (vía Dev, skill `test`) + **commit** + actualiza `map.md`.

Cada gate resuelve algo: el handoff mantiene al Dev enfocado (no explora ni adivina); el **QA humano** evita que la IA siga de largo; el delta ahorra re-explicar; el **commit-vía-Leader** impide que entren cambios sin evaluación de coherencia. **Ningún `git commit` sin el Leader** — y vos siempre tenés la última palabra.

### La anatomía del workspace

Cada trabajo es un monorepo con tres clases de sub-proyecto, sembradas desde material crudo (`resources/`):

- **`business/`** — autoridad de integración: qué sub-proyectos existen, qué expone/consume cada uno, qué decisiones de negocio condicionan el diseño.
- **`design-system/`** — el lenguaje visual que alinea todo lo visual.
- **solutions** (`web/`, `app/`, `backoffice/`, `backend/`…) — el software que materializa el negocio.

Resuelve que cada proyecto no invente su estructura ni su integración por su cuenta: `business/` es la única fuente de **cómo encajan las piezas**; `design-system/` evita que cada pantalla invente su propio lenguaje.

### La memoria por solution

Tres capas con **cadencias distintas**, cada una resolviendo un problema de memoria diferente:

- **Grafo Graphify** *(automático)* — el sobrevuelo del código (estructura y conexiones). Se reconstruye solo post-commit; se consulta con `graphify query|explain|path`. Entender la estructura **sin leer todo el código ni redactarla a mano**.
- **`map.md`** *(curado por el Leader)* — solo lo que el grafo no infiere: infra, integración, convenciones. Más una sección **Evolución**: la síntesis vigente (principios, deuda, ideas parqueadas con motivo) que ninguna task individual posee.
- **`tasks/NNNN-slug/`** *(inmutable)* — brief + tasks + doc por cambio: la crónica del *por qué*, la **memoria absoluta** (append-only).

El Dev **no** consulta map ni grafo — son herramientas de comprensión del Architect/Leader, que destilan los punteros al handoff.

### Skills y reglas transversales

- **Skills** — `implement` y `test` traen **el enfoque**; la **disciplina** (los límites, "no decidas diseño", "parás y escalás si no te alcanza") vive en la persona del Dev. Hay skills bundled (ej. `claude-api`, fuente autoritativa de modelos — se invoca, no se copia, para no memorizar IDs que cambian).
- **Scope de sesión estricto** — se abre la raíz del workspace, y los edits solo bajo ella. Lo **obliga el `settings.json`**, no solo la prosa (ver *Plomería*).
- **Sesgo a la simplicidad** — resolver simple, no robusto; la sobre-ingeniería es deuda.
- **Cross-project** — un cambio que toca varios = una task por proyecto, con vínculo declarado; la integración de fondo queda en `business/`.
- **Toda solución parte de un problema** — si una task no rastrea a `fundamentals/` (o al norte de `business/`), no se construye.

### La plomería — cómo Claude Code lo *obliga*

- **Plugin + marketplace** distribuyen el formato (roles, comandos, skills, templates) desde GitHub; se mantiene una vez y todos consumen la misma versión (por SHA de commit).
- **`settings.json`** hace real el scope: `Edit(/**)` acota la edición a la raíz del workspace, `Read(~/.claude/plugins/**)` deja leer la definición del plugin, un `deny` protege el cache. Todo con **rutas relativas, sin paths de máquina** → portable a cualquier equipo.
- **La confianza del workspace** — los `allow` solo se activan al aceptar el diálogo "Yes, I trust this folder". Nada concede capacidades sin tu consentimiento.

---

## Qué es global y qué es particular

| Global (acá, en el harness) | Particular (en cada workspace) |
|---|---|
| Roles: `agents/` (Architect · Dev · Leader) | `CLAUDE.md` raíz del workspace (sub-proyectos, estado, quirks) |
| Comandos: `/harness:architect` · `/harness:dev` · `/harness:leader` · `/harness:adopt` | `business/` y `design-system/` (definiciones + `resources/`) |
| Skills: `implement`, `test` | `fundamentals/`, grafo Graphify (`graphify-out/`), `map.md` (capa curada + *Evolución*), `tasks/` de cada solution |
| Templates: workspace, solution, shared, task, settings | `.claude/settings.json` de cada repo (habilita el plugin + permisos) |
| El loop y las reglas → [`harness/format.md`](harness/format.md) | El código de cada solución |

> **Regla de oro:** una mejora al formato se hace **acá** y vale para todos. Los particulares van al `CLAUDE.md`/`.claude/` del workspace, **nunca como fork local del harness**.

---

## Estructura del repo y cómo se carga

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

Se carga por **dos vías**:

1. **El plugin** (marketplace remoto → cache): trae **roles, comandos, skills y templates**, con comandos namespaced `/harness:*`. Es lo que instalás una vez por máquina.
2. **`format.md`** (las reglas ambientales de toda sesión): **no lo carga el plugin**. Lo importa un stub `CLAUDE.md` — el que agrupa tus trabajos — por ruta (`@claude/harness/format.md`), y así se hereda en toda sesión abierta debajo. Por eso `format.md` vive en `harness/` pero **no** es un directorio del plugin: es el doc del formato, versionado junto a los roles que lo implementan. Es la **única pieza que necesita el repo presente localmente** (el stub apunta a un archivo, no al cache del plugin).

---

## Instalación y consumo del plugin

### Máquina nueva — paso a paso (primera vez con Claude Code)

**1. Instalá Claude Code y logueate.** Instalá la CLI o la extensión de VSCode/JetBrains (ver la [doc oficial](https://code.claude.com/docs/en/setup)). Corré `claude` (o abrí la extensión): la primera vez abre el navegador para loguearte con tu cuenta de Claude.ai (Pro/Max/Team). Cuando ves `Login successful`, listo.

**2. Instalá el plugin — una vez por máquina.** En una terminal:

```
claude plugin marketplace add Federicogimenez/claude
claude plugin install harness@federicode
```

Queda en el **cache de usuario** (`~/.claude/plugins`) y vale para **todos** tus workspaces — no se instala por proyecto. Con eso los comandos `/harness:*` y los subagentes `architect`/`dev`/`leader` quedan disponibles en toda sesión.

> El repo es **público**: no hace falta ninguna credencial. (Si algún día pasa a privado, configurás git auth una vez con `gh auth setup-git`.)

**3. Instalá Graphify — una vez por máquina.** `uv tool install graphifyy` (necesitás [`uv`](https://docs.astral.sh/uv/)). Ver *Dependencia externa*.

**4. Abrí un workspace y aceptá la confianza.** Abrí la **raíz del workspace** donde vas a trabajar. La primera vez, Claude Code muestra el diálogo de confianza: aceptá **"Yes, I trust this folder"**. Recién ahí se activan los permisos del loop (edición libre en el scope, lectura del plugin); **sin aceptarlo, cada edit y cada lectura del plugin pide permiso** y parece que la sesión "ve toda la máquina". Si el workspace todavía no tiene la anatomía del harness, corré `/harness:adopt`.

**5. A trabajar.** `/harness:architect` arranca el loop.

### Cómo se consume — y reinstalar desde cero

El plugin **vive en GitHub**, no en ninguna carpeta local. Consumirlo es un flujo de dos pasos, **independiente de tener el repo clonado**:

1. **Registrás el marketplace** (`marketplace add`): Claude Code clona el repo a su estado interno (`~/.claude/plugins/marketplaces/`).
2. **Instalás el plugin** (`install`): copia el plugin al **cache** (`~/.claude/plugins/cache/<marketplace>/<plugin>/<versión>/`). Desde ahí lo cargan todas tus sesiones.

Lo que se ejecuta es la copia del cache; la fuente es el repo en GitHub. **Reinstalar desde cero** (borraste un checkout, limpiaste el cache, o algo quedó inconsistente) no depende de ningún archivo local:

```
claude plugin marketplace remove federicode      # quita el registro y desinstala su plugin
claude plugin marketplace add Federicogimenez/claude
claude plugin install harness@federicode
```

El `add` re-clona desde GitHub y el `install` reconstruye el cache. Verificás con `claude plugin marketplace list` o con `/plugin` dentro de una sesión.

> Borrar solo la carpeta local del repo **no afecta** a las sesiones que ya consumen el plugin: leen del cache, no de tu checkout. El checkout local solo importa para el stub de `format.md`, para **mantener** el harness, o para probar cambios con `--plugin-dir`.

**Actualizar:** cada commit pusheado es una versión nueva (por SHA). `claude plugin marketplace update federicode` trae la última — o se baja sola en el auto-update en background al abrir sesiones.

### Alternativa: que el workspace lo pida solo

Si el `.claude/settings.json` del workspace ya trae `extraKnownMarketplaces` (source `github`) + `enabledPlugins` — lo que deja `/harness:adopt` —, al abrir el workspace y aceptar la confianza Claude Code te ofrece agregar el marketplace e instalar el plugin. No hace falta el paso 2 manual.

---

## Adoptar el harness en un trabajo

Dentro del workspace (nuevo o existente): **`/harness:adopt`**. Siembra la anatomía desde `templates/` — o migra un formato viejo conservando lo particular (briefs, docs, standards → `resources/`; `map.md`/`memory.md` viejos → el `map.md` nuevo con su *Evolución*) — instala el `settings.json` portable y el hook de Graphify, y deja pendiente para el **Architect** (sembrar definiciones desde `resources/`) y el **Leader** (completar `map.md`). No commitea: deja el árbol listo para que vos cierres vía Leader.

---

## Cómo se usa

Una vez adoptado, el ciclo diario:

- **`/harness:architect`** *(rol por defecto)* — entrás a diseñar. Entiende la raíz, investiga, pesa la task y arma el handoff del Dev. Conduce hasta el QA y **para**.
- **`/harness:dev`** (o subagente `dev`) — ejecuta una skill + un handoff, sin decidir diseño. Lo invoca el Architect (implementar) y el Leader (pruebas).
- **`/harness:leader`** *(chat dedicado)* — lo disparás vos para commitear: evalúa coherencia, mantiene `map.md`/integración, corre pruebas vía Dev y **commitea**. Es el gate.

El humano hace el **QA visual** entre el Architect y el Leader, y siempre tiene la última palabra.

---

## Dependencia externa: Graphify

El **grafo** de la memoria por solution lo genera Graphify, una herramienta externa instalada por máquina (`uv tool install graphifyy`). La siembra por solution la ejecuta `/harness:adopt` (fuente canónica del setup); los comandos de consulta viven en el `map.md` de cada solution (sección *El grafo*). Cubre **solo el código** — el `.graphifyignore` del template excluye `.claude/`: es el sobrevuelo optimizado del code base; el detalle y el porqué viven en `map.md` y `tasks/`.

**Mantenimiento post-commit:** el hook nativo (`graphify hook install`) asume un repo por proyecto — en el monorepo del workspace instalaría un solo hook para un solo grafo y dejaría los demás sin mantener. El harness trae un **wrapper monorepo** (`harness/templates/workspace/hooks/post-commit`) que reconstruye el grafo de cada solution tocada por el commit; `/harness:adopt` lo instala en `.git/hooks/post-commit` del workspace.

---

## Topología git y modelo de sesión

Cada workspace es **un solo repo git** (monorepo de sub-proyectos): el Leader commitea en la raíz, y su `.claude/settings.json` vale para todos los sub-proyectos. El `graphify-out/` de cada solution se regenera local y va al `.gitignore`. Este repo `claude/` (el harness) lleva su **propio** repo, separado de los workspaces.

Se trabaja **abriendo la raíz de cada workspace**, nunca la carpeta que los agrupa — el scope de la sesión es estricto a ese workspace. Para **mantener el harness**, la sesión se abre en la **raíz de este repo**. El `settings.json` lo hace cumplir: edita solo bajo la raíz del workspace (`Edit(/**)`) y deniega editar el cache del plugin (`Edit(~/.claude/plugins/**)`) — un `deny` gana sobre cualquier `allow`, así que una sesión de workspace no toca el harness ni por accidente.

---

## Mantenimiento

- **Guardián:** el rol Leader custodia la frontera formato↔particular y la config del harness — ver `agents/leader.md`, sección *Guardián del harness*.
- **Cómo propagan los cambios: commiteás y pusheás.** El plugin instalado es una **copia del cache versionado**, no tu working tree vivo. Cada commit pusheado es una versión nueva (por SHA); los workspaces la reciben con `marketplace update` o por el auto-update. **Sin `git push`, el cambio no llega a ninguna máquina.**
- **Probar cambios sin publicar:** abrí el harness con `claude --plugin-dir ./harness` — carga tu working tree y pisa la versión instalada **solo en esa sesión**, sin commit ni push. `/reload-plugins` recarga sin reiniciar.
- **El commit + push es el gate.** Dejar el working tree sucio no propaga nada y pierde la trazabilidad del formato — que es todo lo que este repo aporta.
- **No declares `version`** en `plugin.json` ni en la entrada del marketplace: un `version` fijo **pinea** el plugin y los commits nuevos dejan de propagarse sin aviso. Sin el campo, la versión cae al SHA del commit. (La doc además desaconseja declararlo en los dos lados: el de `plugin.json` gana en silencio.)

---

## Hacer un fork (tu propio harness)

Si querés partir de este harness para armar el tuyo:

1. **Cloná o forkeá** el repo a tu propio GitHub (`tu-usuario/tu-repo`).
2. **`.claude-plugin/marketplace.json`** — cambiá `name` (el nombre del marketplace, hoy `federicode`) y `owner`. Ese `name` es con el que se instala: `<plugin>@<name>`.
3. **`harness/.claude-plugin/plugin.json`** — actualizá `author`. El `name` del plugin (`harness`) podés dejarlo o cambiarlo; si lo cambiás, cambia el namespace de los comandos (`/tu-plugin:*`).
4. **`harness/templates/settings/settings.json`** — apuntá `extraKnownMarketplaces.<tu-marketplace>.source.repo` a `tu-usuario/tu-repo`, y `enabledPlugins` a `<plugin>@<tu-marketplace>`. (Los permisos `Edit(/**)` / `Read(~/.claude/plugins/**)` son portables — no los toques.)
5. **El stub de `format.md`** — el `CLAUDE.md` que agrupa tus trabajos importa `format.md` por ruta. Ajustá la ruta a donde clones tu harness, y mantené ahí un checkout local (es la pieza que no llega por el plugin).
6. **Publicá** con `git push` y consumí como en *Instalación*, con tu repo y tu marketplace.

Revisá `format.md` y los `agents/` por menciones de marca ("federicode") si querés renombrar. No declares `version` — dejá que el SHA versione.
