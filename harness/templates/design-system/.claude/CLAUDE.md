# design-system — Lenguaje de marca

> Se carga al trabajar dentro de `design-system/`. Hereda el formato de trabajo global del harness federicode (`/harness:format`) y los particulares del workspace ([../../CLAUDE.md](../../CLAUDE.md)). Es un **aligner**: no tiene objetivo propio, existe para que ninguna solution re-derive por su cuenta un valor visual ni un modo de hablar.

## Qué es
<El lenguaje de marca común — **visual** (color, tipografía, forma, iconografía, primitivos de UI) y **verbal** (persona, registro, glosario del producto, cómo hablan errores y estados vacíos) — y a qué solutions alinea. Su norte es que ninguna re-derive por su cuenta un valor visual ni un modo de hablar.>

> La voz entra acá por la misma razón que el color: si cada solution decide sola si trata de *vos* o de *tú*, la marca se rompe igual que con dos azules distintos. Pero **su origen no es este proyecto**: el posicionamiento y la audiencia se deciden en [business/](../../business/.claude/CLAUDE.md) y acá se traducen a reglas aplicables. La **densidad e intención** del texto (telegráfico vs. persuasivo) las fija cada solution en su `fundamentals/`, citando.

## Dónde vive el canon
> La regla `canon` / `spec` — los dos estados, cuándo transiciona una capa, quién lo registra y los tres tests del gate — está definida **una sola vez** en [`manual/README.md`](../manual/README.md). Acá va solo el particular de este workspace, y el detalle capa por capa en [`map.md`](map.md) § *Dónde vive el canon*.

**<Fuente de verdad> es el canon.** <Dónde está implementado hoy — código real en producción; las capas todavía sin implementar tienen su canon en su página del `manual/`.>

`resources/` es material histórico que sirvió para *llegar* hasta ahí — **no es canon vigente**. Ante cualquier conflicto, gana el canon; el desvío se **registra como divergencia en [`map.md`](map.md) § *Evolución***, con su motivo. Nunca se "corrige" el canon para que matchee el papel, ni se copia un valor de `resources/` que lo contradiga.

## No negociables
> Enunciados verificables, en imperativo, con cómo se comprueban. MÁXIMO 4. Prueba de admisión: si el enunciado no cambiaría una decisión del Architect ni atraparía una deriva en el gate del Leader, no va acá — es descripción, y la descripción vive en las otras secciones.

- **<Enunciado en imperativo>.** Se verifica: `<cómo se comprueba en un diff o en el producto>`.

Los lee el Architect al diseñar y el Leader en el gate. El Dev nunca los lee: le llegan citados en el handoff.

## Quién lo consume

| Solution | Qué consume | Cómo |
|---|---|---|
| <solution/> | <qué capas del lenguaje toma — visuales y/o `voz`> | <mecanismo: tokens directos, página del `manual/`, licencia propia declarada...> |

Esta tabla es la lista de a quién nuclea `design-system/`: sin ella no se sabe a quién afecta un cambio de canon, y es la que decide qué capas del `manual/` merecen existir. Cuando cambia, el gate del Leader revisa el `fundamentals/` de cada fila.

## El manual — el output de las tasks de este proyecto
> Una task acá **no produce un manual: produce canon.** El manual es la acumulación, no el entregable.

El lenguaje aplicado vive en [`manual/`](../manual/) — **fuera de `.claude/`**, para consultarse bajo demanda y no pagarse en tokens cada sesión. Una página por capa (`color.md`, `tipografia.md`, `voz.md`…), duplicando `manual/_plantilla-capa.md`. Cuándo nace una página, cuánto ocupa, cómo transiciona de `canon` a `spec` y qué tests corre el Leader: todo en [`manual/README.md`](../manual/README.md).

## Estado
🌱 Bootstrap.

## Memoria del proyecto — dos cadencias
Como no tiene código, no tiene grafo: quedan las dos capas de prosa. El **canon** (el `manual/`) no es memoria — es lo que este proyecto expone.

| Capa | Cadencia | Qué guarda |
|---|---|---|
| [map.md](map.md) | curada por el **Leader**, por commit | Dónde vive el canon, qué expone, quién lo consume + § *Evolución* (decisiones, divergencias con `resources/`, ideas parqueadas) |
| [tasks/](tasks/) | **inmutable**, por cambio | La crónica del *por qué* |

El lenguaje aplicado, una página por capa → [manual/](../manual/).

> Su intake es la carpeta `resources/` (material crudo — visual y verbal — que carga el humano; el Architect lo procesa en las definiciones de este archivo y en las páginas del `manual/`). Es material histórico **inmutable**: no se edita para alinearlo con el canon — las diferencias se registran en `map.md` (§ *Evolución*). Las ideas descartadas o pospuestas quedan ahí con motivo.
