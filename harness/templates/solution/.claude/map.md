# Mapa — <proyecto>

> La **capa curada** sobre el grafo. La estructura del código (qué hay, dónde vive, cómo se conecta) **no se redacta acá**: vive en el **grafo Graphify** del proyecto, que se reconstruye solo tras cada commit. Acá va únicamente lo que el grafo **no puede inferir**, más — al final — la **síntesis evolutiva** que ninguna task individual posee. Lo mantiene el **Leader** en cada commit — denso, veraz, corto.

## El grafo (code map automático)

- **Scope y función:** el sobrevuelo (mind map) del **código** de la solution — el `.graphifyignore` de la raíz excluye `.claude/`. Sirve para análisis optimizado de estructura y conexiones; el detalle y el porqué viven en este `map.md` y en `tasks/`.
- Consultas: `graphify query "<keywords>"` · `graphify explain "<archivo o símbolo>"` · `graphify path <A> <B>` · `graphify affected "<X>"` (qué impacta X).
- Las consultas matchean **labels de nodos** (archivos, símbolos, títulos): usá términos del código, no preguntas largas en lenguaje natural.
- Vive en `graphify-out/` (raíz del proyecto, ignorado por git — se regenera local).
- Lo reconstruye el hook post-commit del **workspace** (wrapper monorepo del harness, instalado por `/harness:adopt`; solo AST, sin costo; log en `~/.cache/graphify-rebuild.log`). Si el grafo no existe o está viejo: `graphify update .`

## Infra del proyecto
_(build, cómo se corre, framework y ubicación de pruebas, deploy, qué NO debe subir al repo)_

## Integración con otros proyectos
_(qué expone/consume respecto de los demás sub-proyectos — coherente con `business/.claude/CLAUDE.md`)_

## Convenciones de ubicación
_(dónde va lo nuevo: qué patrón de carpetas/naming sigue este proyecto — solo lo que no sea obvio desde el grafo)_

## Evolución
> La **síntesis vigente** que ninguna task individual posee. La crónica del *por qué* decisión-por-decisión **no va acá**: vive en los `brief.md` de `tasks/` (historial inmutable). Acá solo lo que emergió del recorrido y sigue vigente. Evolutiva, no acumulativa: se fusiona lo redundante, se poda lo obsoleto.

### Principios
_(ninguno registrado todavía)_

### Deuda conocida
_(ninguna)_

### Ideas parqueadas
> Ideas pesadas por el Architect contra esta memoria y descartadas o pospuestas — **con motivo**, para no re-litigarlas.

| Idea | Veredicto / motivo | Fecha |
|------|--------------------|-------|
| — | — | — |
