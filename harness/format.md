# federicode — Formato de trabajo global

Este archivo es la **fuente de verdad del formato de trabajo** común a todos los trabajos, y vive en el plugin (`harness@federicode`). No se inyecta como CLAUDE.md ambiental (los plugins no lo permiten): se carga **on-demand** con `/harness:format`, que lo lee desde el plugin — **sin necesidad de clonar el harness**. La orientación mínima de cada workspace la siembra `/harness:adopt` en su `CLAUDE.md`. **Lo global vive acá; lo particular en cada workspace.** Una mejora al formato se hace en el harness, nunca en una copia local.

## Anatomía de un trabajo (workspace)

Cada carpeta de primer nivel en `trabajos/` es un **workspace**: un conjunto de sub-proyectos interdependientes con su `CLAUDE.md` raíz (solo particulares) — **por default un monorepo** (un solo repo git; la topología recomendada), con variante **poly-repo** cuando un proyecto lo justifica (ver `/harness:adopt`, topología git). Dos shared lo siembran y alinean, y las soluciones de software lo materializan:

- **`business/`** — modelo de negocio; su `.claude/CLAUDE.md` es la **autoridad de integración** (qué sub-proyectos existen, qué expone/consume cada uno, qué decisiones de negocio condicionan el diseño).
- **`design-system/`** — el **lenguaje visual** que alinea todo; toda task de diseño se ancla acá.
- **Solutions** (`web/`, `app/`, `backoffice/`, `backend/`…): skeleton completo `fundamentals` + memoria (grafo + `map.md`) + `tasks`. Se arranca con las que `business/` justifique.

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
5. **Vos disparás al Leader** cuando decidís commitear: gate de coherencia proporcional al peso + pruebas (vía Dev, skill `test`) + commit + actualiza `map.md` (capa curada + su *Evolución*).

**Gate obligatorio:** ningún `git commit` sin el Leader. Vos siempre tenés la última palabra — el Leader propone, no impone.

## Reglas transversales

- **Scope de sesión estricto:** se trabaja abriendo la **raíz del workspace** (carpeta hermana de `claude/`, ej. `trabajos/dedo/`), nunca `trabajos/`. **Ningún archivo que la sesión cree vive fuera de esa raíz** — y esta regla **gana sobre cualquier default de scratchpad, carpeta temporal o directorio de trabajo que proponga el entorno**: si el entorno ofrece un `/tmp`, un `AppData\Local\Temp\…` o similar "para archivos temporales", **no se usa**. Escribir afuera deforma el sistema del host y rompe los links que la sesión emite: apuntan a rutas que no existen para nadie más. *No son excepción* las herramientas con destino propio (git y sus hooks, `uv tool install`, la caché de plugins) — eso no lo crea el agente. El harness — **este archivo incluido** — se edita solo en sesiones abiertas en `trabajos/claude/`, su propia raíz.
- **`.scratch/` — artefactos derivados de sesión:** todo lo desechable que una sesión necesite escribir (borradores, salidas intermedias, scripts de una sola vez, dumps de análisis) va a **`.scratch/` en la raíz del workspace**, gitignorado. Es la **ubicación canónica**, y existe porque sin ella la regla anterior es una prohibición sin destino — y un default del entorno, que sí nombra una carpeta concreta, la gana por concreción. **Frontera:** `.scratch/` es solo para lo que se puede borrar sin perder nada; lo que es **entregable** — la documentación de una task — va a `tasks/NNNN-slug/doc/` y **se versiona**. Ante la duda, es `doc/`.
- **El scope se sostiene por norma, no por permisos:** el `settings.json` del workspace respalda lo de arriba (`Edit`/`Write` anclados bajo la raíz, temporales del sistema y caché de plugins denegados), pero el respaldo es **parcial**: `Bash` escribe donde quiera y ninguna regla de permisos lo ataja por path. La norma manda; el archivo de permisos solo encarece equivocarse.
- **Sesgo a la simplicidad:** resolver simple, no robusto. Sobre-ingeniería es deuda; robustez solo cuando es muy necesaria y justificada.
- **Cross-project:** un cambio que toca varios sub-proyectos = una task por proyecto, con vínculo declarado en cada `brief.md`; la integración de fondo queda en `business/.claude/CLAUDE.md`.
- **Toda solución parte de un problema:** si una task no rastrea a `fundamentals/` (o al norte de `business/`), no se construye.
- **Memoria por solution:** **grafo Graphify** (code map automático — estructura y conexiones, se reconstruye post-commit; se consulta con `graphify query|explain|path`, uno por solution) + `map.md` (capa curada: infra, integración, convenciones que el grafo no infiere, y al final una sección *Evolución* con la síntesis vigente — principios, deuda, ideas parqueadas con motivo — que ninguna task individual posee). Más `tasks/NNNN-slug/` (historial **inmutable**: `brief.md` + `tasks.md` + `doc/` — la crónica del *por qué* por cambio). El Dev no consulta map ni grafo — recibe punteros destilados en el handoff.
- **Skills:** internas (`implement`, `test` — solo el enfoque; la disciplina vive en la persona del Dev), instaladas (marketplace de terceros) y bundled (ej. `claude-api`, fuente autoritativa de modelos — se invoca, no se copia).
- **Templates:** workspace, solution, shared, task y settings viven en el harness (`trabajos/claude/harness/templates/`). Task nueva = copiar `templates/task/` y numerar secuencial (`0001-`, `0002-`, …).
