# Fundamentals — <proyecto>

> El **norte** del proyecto: qué problema resuelve, con qué objetivo y hasta dónde llega. Todo lo que hace el Architect se mide contra esto; el Leader lo usa para juzgar coherencia.

**No es memoria.** La memoria son tres cadencias (grafo · `map.md` · `tasks/`) y registra lo que pasó; el norte es el criterio contra el que se las mide. Por eso no cambia por deriva: evoluciona **deliberadamente** — el **Architect propone** el cambio (porque la necesidad de la etapa cambió), el **Leader ratifica** que es coherente. Cada cambio se anota abajo.

## Los tres lentes
- [problems.md](problems.md) — los problemas / necesidades de fondo.
- [objectives.md](objectives.md) — qué buscamos lograr.
- [scope.md](scope.md) — qué entra y qué queda afuera (ahora).

## Cómo se ancla a los aligners

- **Se cita, no se copia.** Link relativo + comilla textual de la definición del aligner (`business/.claude/CLAUDE.md` o `design-system/.claude/CLAUDE.md`). No se reformula ni se resume: una reformulación es un fork que todavía no se nota.
- **La división:** `fundamentals/` responde *qué problema resuelve ESTA solution*; el aligner responde *qué vale para TODAS*. Si necesitás transcribir un enunciado entero, es señal de que ese enunciado pertenece al aligner, no acá.
- **El caso del texto, que se confunde seguido:** la **voz** (persona, registro, glosario) vale para todas → vive en `design-system/manual/voz.md` y se **cita**. La **densidad e intención** son de esta solution — un backoffice es telegráfico, una landing persuade → se declaran acá. Contradecir la voz por cuenta propia no es una decisión local: es una divergencia, y va al `map.md` del aligner.
- **La cita es lo que hace rastreable la propagación:** por el link se sabe quién depende de qué cuando el aligner cambia.

## Evolución
| Fecha | Cambio | Por qué (qué necesidad de etapa cambió) | Ratificado |
|-------|--------|------------------------------------------|------------|
| — | Creación del esqueleto | Bootstrap del proyecto | — |
