# federicode — Formato de trabajo global

Este archivo es la **fuente de verdad del formato de trabajo** común a todos los trabajos, y vive en el plugin (`harness@federicode`). No se inyecta como CLAUDE.md ambiental (los plugins no lo permiten): se carga **on-demand** con `/harness:format`, que lo lee desde el plugin — **sin necesidad de clonar el harness**. La orientación mínima de cada workspace la siembra `/harness:adopt` en su `CLAUDE.md`. **Lo global vive acá; lo particular en cada workspace.** Una mejora al formato se hace en el harness, nunca en una copia local.

## Cómo encajan las partes

1. **Cada regla es un contrapeso.** Un ejecutor de IA puede hacer casi todo y por eso falla de formas predecibles: sobre-ingeniería, perder el porqué, desviarse del problema, tocar fuera de alcance, exigir que le re-expliquen todo cada sesión. Cada definición de este formato responde a uno de esos modos de falla — no es burocracia, es la mínima estructura que hace repetible trabajar con IA entre muchos proyectos.
2. **Los roles se equilibran; el humano corta.** Architect crea y Leader evalúa con **contextos independientes** — un solo agente que diseña, ejecuta y aprueba se auto-convence. El Dev ejecuta neutral entre los dos. El humano es el **único hilo que atraviesa los tres**: hace el QA, dispara al Leader y tiene la última palabra.
3. **Los aligners nuclean; las solutions materializan.** `business/` y `design-system/` definen lo transversal una sola vez; los `fundamentals/` de cada solution lo **citan**, nunca lo copian. El cambio viaja en las dos direcciones: una task en una solution re-deriva el aligner, y un cambio en el aligner obliga a revisar el `fundamentals/` de cada consumidor declarado. Eso es lo que sostiene la homogeneidad entre proyectos.
4. **El handoff es la membrana.** Todo lo que el Dev necesita se le **destila**; nada se le pide que busque. Por eso no lee `map.md`, ni el grafo, ni los aligners: su contexto acotado es una ventaja, no una carencia — y quien destila (Architect o Leader) es quien ya entendió el problema.
5. **La memoria tiene tres cadencias porque ninguna capa puede sostener a la otra.** El grafo se reconstruye solo (el código cambia más rápido que cualquier prosa); el `map.md` lo cura el Leader por commit (lo que el grafo no puede inferir, más la síntesis vigente); las `tasks/` son inmutables por cambio (el porqué, que no se re-escribe nunca). El **norte** (`fundamentals/`) **no es una cuarta capa de memoria**: es el criterio contra el que se mide la memoria, y por eso no cambia por deriva sino por decisión (el Architect propone, el Leader ratifica).

## Anatomía de un trabajo (workspace)

Un **workspace** es la carpeta raíz de un trabajo — el conjunto de sub-proyectos interdependientes que se abren juntos en una sesión. Dónde vive en el disco es irrelevante: no lo define su ubicación ni la carpeta que lo agrupa (eso cambia de máquina en máquina), lo define tener un `CLAUDE.md` raíz (solo particulares) más la anatomía de abajo — **por default un monorepo** (un solo repo git; la topología recomendada), con variante **poly-repo** cuando un proyecto lo justifica (ver `/harness:adopt`, topología git). Dos **aligners** lo siembran y alinean, y las **solutions** lo materializan:

- **`business/`** — modelo de negocio; su `.claude/CLAUDE.md` es la **autoridad de integración** (qué sub-proyectos existen, qué expone/consume cada uno, qué decisiones de negocio condicionan el diseño).
- **`design-system/`** — el **lenguaje de marca** que alinea todo: **visual** (color, tipografía, forma, iconografía) y **verbal** (persona, registro, glosario, cómo hablan errores y vacíos). Toda task de diseño o de copy se ancla acá. Su output no es un manual sino **canon**: una página por capa en `design-system/manual/` (fuera de `.claude/`, consulta bajo demanda), que nace como `canon` y se degrada a `spec` cuando la capa se implementa en código — un valor nunca vive en dos lados. La regla completa (estados, transición, quién la registra, los tres tests) se define **una sola vez** en `design-system/manual/README.md`.
- **Solutions** (`web/`, `app/`, `backoffice/`, `backend/`…): el norte (`fundamentals/`) + memoria (grafo + `map.md` + `tasks/`). Se arranca con las que `business/` justifique.

`business/` y `design-system/` son los **aligners** del workspace (proyectos sin objetivo propio: existen para que los demás no re-deriven lo transversal). Se siembran desde su carpeta **`resources/`** (material crudo que carga el humano; el Architect lo procesa en definiciones, el Leader las mantiene veraces). La cadena de la marca los encadena a los tres: **`business/` decide** (posicionamiento, audiencia), **`design-system/` lo hace aplicable** (reglas sin criterio estético), **las solutions lo citan** — y solo fijan lo suyo: la densidad e intención del texto, no la voz.

> **Workspace sin esta anatomía** (proyecto viejo o nuevo): no improvises un formato local **ni copies templates a mano** — `/harness:adopt` es la **única vía de siembra**, porque la anatomía solo queda consistente con sus permisos, `.gitignore`, topología git y grafo sembrados a la vez.

## Los roles

Dos enfoques **opuestos** balancean el trabajo — uno **crea**, el otro **evalúa** — y una mano neutral **ejecuta** entre ambos. Las personas viven en el plugin (`harness@federicode`, carpeta `agents/`), no en cada repo.

| Rol | Carácter | Hace | Entrada |
|---|---|---|---|
| **Architect** | Creativo, filósofo de la raíz | Procesa resources, investiga, diseña, pesa la task, arma el handoff del Dev | `/harness:architect` (rol por defecto) |
| **Dev** | Ejecutor neutral, sin opinión de diseño | Toma una skill (enfoque) + handoff y ejecuta dentro del scope | `/harness:dev`, o como subagente **`harness:dev`** (con namespace: el nombre pelado no resuelve) |
| **Leader** | Conservador, guardián | Gate de coherencia, mantiene el mapa curado y la integración, escribe la `doc/`, **único que commitea** | `/harness:leader` en chat dedicado |

Architect y Leader mantienen contexto y decisiones independientes: por eso se equilibran. El Dev no decide diseño — sirve a ambos.

## El loop

1. **Vos ↔ Architect** entienden la raíz → investiga → diseña. Pesa la task (**liviana/completa**) y arma el **handoff** autosuficiente (instrucción condensada + scope de archivos + punteros — el Dev no lee el `map.md`).
2. **Dev** ejecuta sobre la skill + el handoff, con contexto solo de ejecución, todos los edits de corrido.
3. **Vos hacés QA visual.** El Architect surfacea el resultado y **para acá** — el Leader no se encadena.
4. Ajustes del QA → **corrección por delta al mismo Dev** (sin reboot) → volvés a QA, hasta tu OK.
5. **Vos disparás al Leader** cuando decidís commitear: gate de coherencia proporcional al peso + pruebas (vía Dev, skill `test`) + escribe la `doc/` de la task (verificando la implementación contra el brief y las pruebas) + commit + actualiza `map.md` (capa curada + su *Evolución*) + cierra la task.

En el loop Architect↔Dev **solo se marca `tasks.md`**; la `doc/` no se escribe ahí. La escribe el Leader al final, cuando ya verificó qué quedó realmente construido — así documenta lo que hay, no lo que se pensaba hacer.

**Gate obligatorio:** ningún `git commit` sin el Leader. Vos siempre tenés la última palabra — el Leader propone, no impone.

## Reglas transversales

- **Scope de sesión estricto:** se trabaja abriendo la **raíz del workspace**. **Ningún archivo que la sesión cree vive fuera de esa raíz** — y esta regla **gana sobre cualquier default de scratchpad, carpeta temporal o directorio de trabajo que proponga el entorno**: si el entorno ofrece un `/tmp`, un `AppData\Local\Temp\…` o similar "para archivos temporales", **no se usa**. Escribir afuera deforma el sistema del host y rompe los links que la sesión emite: apuntan a rutas que no existen para nadie más. *No son excepción* las herramientas con destino propio (git y sus hooks, `uv tool install`, la caché de plugins) — eso no lo crea el agente. El harness — **este archivo incluido** — se edita solo en una sesión abierta en la raíz del repo del harness.
- **`.scratch/` — artefactos derivados de sesión:** todo lo desechable que una sesión necesite escribir (borradores, salidas intermedias, scripts de una sola vez, dumps de análisis) va a **`.scratch/` en la raíz del workspace**, gitignorado. Es la **ubicación canónica**, y existe porque sin ella la regla anterior es una prohibición sin destino — y un default del entorno, que sí nombra una carpeta concreta, la gana por concreción. **Frontera:** `.scratch/` es solo para lo que se puede borrar sin perder nada; lo que es **entregable** — la documentación de una task, que escribe el Leader en el gate — va a `tasks/NNNN-slug/doc/` y **se versiona**. Ante la duda, es `doc/`.
- **El scope se sostiene por norma, no por permisos:** el `settings.json` del workspace respalda lo de arriba (`Edit`/`Write` anclados bajo la raíz, temporales del sistema y caché de plugins denegados), pero el respaldo es **parcial**: `Bash` escribe donde quiera y ninguna regla de permisos lo ataja por path. La norma manda; el archivo de permisos solo encarece equivocarse.
- **Sesgo a la simplicidad:** resolver simple, no robusto. Sobre-ingeniería es deuda; robustez solo cuando es muy necesaria y justificada.
- **Cross-project:** un cambio que toca varios sub-proyectos = una task por proyecto, con vínculo declarado en cada `brief.md`; la integración de fondo queda en `business/.claude/CLAUDE.md`.
- **Aligners — referencia, no copia.** Los `fundamentals/` citan a `business/`/`design-system/` con link y comilla textual, nunca transcriben; y un cambio EN un aligner obliga a revisar el `fundamentals/` de cada consumidor declarado (gate del Leader). Es lo que sostiene la homogeneidad entre solutions.
- **Toda solución parte de un problema:** si una task no rastrea a `fundamentals/` (o al norte de `business/`), no se construye.
- **Memoria por solution — tres cadencias:** **grafo Graphify** (code map automático — estructura y conexiones, se reconstruye post-commit; se consulta con `graphify query|explain|path`, uno por solution) + `map.md` (capa curada: infra, integración, convenciones que el grafo no infiere, y al final una sección *Evolución* con la síntesis vigente — principios, deuda, ideas parqueadas con motivo — que ninguna task individual posee) + `tasks/NNNN-slug/` (historial **inmutable**: `brief.md` + `tasks.md` + `doc/` — la crónica del *por qué* por cambio). El **norte** (`fundamentals/`) no es una de las tres: es el criterio contra el que se miden. El Dev no consulta map ni grafo — recibe punteros destilados en el handoff.
- **Skills:** internas (`implement`, `test` — solo el enfoque; la disciplina vive en la persona del Dev), instaladas (marketplace de terceros) y bundled (ej. `claude-api`, fuente autoritativa de modelos — se invoca, no se copia).
- **Templates:** workspace, solution, business, design-system, task y settings viven en el harness (`harness/templates/`). Cómo nace, avanza y se cierra una task — numeración, qué se copia, estados y quién los mueve — está definido **una sola vez** en `templates/task/README.md`; acá no se repite.
- **Una definición, un dueño.** Este archivo es **el spec**: enuncia cada regla una sola vez. La persona del rol que la ejecuta (`agents/`) dice **cómo se opera** desde ese asiento, y el template la recuerda **en el punto de uso** — nada más. Si una regla aparece una cuarta vez, ya divergió o va a divergir. Es la misma disciplina de *referencia, no copia* que se les exige a los `fundamentals/`, aplicada al propio formato.
- **El repo del harness se gobierna distinto** — se relee entero cada sesión, así que no lleva `tasks/`: una decisión se registra como propiedad del texto vigente, no como historia sobre él; el porqué crudo va al mensaje de commit (`git log` es su crónica) y el gate es un barrido de coherencia completo.
