# <proyecto> — Solution

> Se carga al trabajar dentro de `<proyecto>/`. Hereda el workflow global del harness federicode (roles, loop) y los particulares del workspace ([../../CLAUDE.md](../../CLAUDE.md)).

## Norte
La necesidad raíz, objetivos y alcance viven en [fundamentals/](fundamentals/). Su rol dentro del negocio y cómo se integra con los demás → [business/.claude/CLAUDE.md](../../business/.claude/CLAUDE.md).

## Estado
🌱 Bootstrap. Stack por elegir — decisión del Architect basada en investigación, registrada en la primera task y coherente con las convenciones cross-project.

## Memoria del proyecto
- Norte (problemas, objetivos, alcance) → [fundamentals/](fundamentals/)
- Code map (estructura y conexiones, automático) → **grafo Graphify** (`graphify query|explain|path`)
- Capa curada (infra, integración, convenciones) → [map.md](map.md)
- Evolución histórica (decisiones, principios, deuda, ideas parqueadas) → [memory.md](memory.md)
- Historial de tasks → [tasks/](tasks/)

## Antes de leer todo el código
Consultá el **grafo** para entender estructura y `map.md` para lo curado. Si no alcanzan para tu task, es señal de que el Leader debe refrescar el grafo (`graphify update .`) o enriquecer el map.
