# <proyecto> — Solution

> Se carga al trabajar dentro de `<proyecto>/`. Hereda el formato de trabajo global del harness federicode (roles, loop, reglas — `/harness:format`) y los particulares del workspace ([../../CLAUDE.md](../../CLAUDE.md)).

## Norte — contra qué se mide todo
La necesidad raíz, objetivos y alcance viven en [fundamentals/](fundamentals/). **No es memoria**: es el criterio contra el que se mide la memoria, y por eso no cambia por deriva sino por decisión — el Architect propone, el Leader ratifica.

Su rol dentro del negocio y cómo se integra con los demás → [business/.claude/CLAUDE.md](../../business/.claude/CLAUDE.md).

## Memoria del proyecto — tres cadencias
Ninguna capa puede sostener a la otra; por eso son tres y cada una se actualiza a su ritmo.

| Capa | Cadencia | Qué guarda |
|---|---|---|
| **Grafo Graphify** — `graphify query` · `explain` · `path` | automática, post-commit | Estructura y conexiones del **código** |
| [map.md](map.md) | curada por el **Leader**, por commit | Lo que el grafo no infiere: infra, integración, convenciones — y § *Evolución*, la síntesis vigente (principios, deuda, ideas parqueadas) |
| [tasks/](tasks/) | **inmutable**, por cambio | La crónica del *por qué*: `brief.md` + `tasks.md` + `doc/` |

## Estado
🌱 Bootstrap. Stack por elegir — decisión del Architect basada en investigación, registrada en la primera task y coherente con las convenciones cross-project.

## Antes de leer todo el código
Consultá el **grafo** para entender estructura y `map.md` para lo curado. Si no alcanzan para tu task, es señal de que el Leader debe refrescar el grafo (`graphify update .`) o enriquecer el map.

> El **Dev no abre ninguna de las tres**: recibe punteros destilados en su handoff. El grafo y el map son herramientas de comprensión del Architect y del Leader.
