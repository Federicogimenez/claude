# Mapa — <proyecto>

> La **capa curada** sobre el grafo. La estructura del código (qué hay, dónde vive, cómo se conecta) **no se redacta acá**: vive en el **grafo Graphify** del proyecto, que se reconstruye solo tras cada commit. Acá va únicamente lo que el grafo **no puede inferir**. Lo mantiene el **Leader** en cada commit — denso, veraz, corto.

## El grafo (code map automático)

- Consultas: `graphify query "<keywords>"` · `graphify explain "<archivo o símbolo>"` · `graphify path <A> <B>` · `graphify affected "<X>"` (qué impacta X).
- Las consultas matchean **labels de nodos** (archivos, símbolos, títulos): usá términos del código, no preguntas largas en lenguaje natural.
- Vive en `graphify-out/` (raíz del proyecto, ignorado por git — se regenera local).
- Lo reconstruye el hook post-commit (`graphify hook install`, solo AST, sin costo). Si el grafo no existe o está viejo: `graphify update .`

## Infra del proyecto
_(build, cómo se corre, framework y ubicación de pruebas, deploy, qué NO debe subir al repo)_

## Integración con otros proyectos
_(qué expone/consume respecto de los demás sub-proyectos — coherente con `business/.claude/CLAUDE.md`)_

## Convenciones de ubicación
_(dónde va lo nuevo: qué patrón de carpetas/naming sigue este proyecto — solo lo que no sea obvio desde el grafo)_
