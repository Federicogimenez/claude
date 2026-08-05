# Manual — design-system

> El **lenguaje de marca aplicado**: una página por capa. Vive **fuera de `.claude/`** a propósito — se consulta bajo demanda y no se paga en tokens cada sesión. Lo escribe el **Architect** al resolver una task; el **Leader** lo mantiene veraz en el gate.

Esto **no** es un manual de marca de agencia. No lleva rationale, moodboard ni storytelling: eso es material de `resources/` (histórico) o el `brief.md` de la task que lo decidió (crónica). Acá va solo lo que otro proyecto necesita para **aplicar** el lenguaje sin re-derivarlo.

## Qué es una capa

Una unidad del lenguaje que una solution podría re-derivar mal por su cuenta.

- **Visuales:** `color`, `tipografia`, `espaciado`, `forma`, `iconografia`, `movimiento`, `componentes`.
- **Verbal:** `voz` (ver [voz.md](voz.md) — viene sembrada porque su forma no es una tabla de tokens).

Un archivo por capa, `<capa>.md`, duplicando [_plantilla-capa.md](_plantilla-capa.md). La plantilla no se completa ni se borra: se copia.

## Canon o spec — la regla que evita la doble fuente de verdad

> **Esta es la definición.** El resto del workspace la referencia y no la repite: [`.claude/CLAUDE.md`](../.claude/CLAUDE.md) declara qué es el canon *en este workspace*, [`map.md`](../.claude/map.md) lleva el estado capa por capa, y la persona del Leader la aplica en el gate.

- Mientras la capa **no está implementada**, su página **es el canon**: los valores literales viven acá.
- Cuando la capa **se implementa** (tokens en código), el código pasa a ser el canon y la página **se degrada a `spec`**: los valores se reemplazan por el puntero al archivo real. Sobreviven la regla de aplicación, los anti-usos y el origen — lo que el código no puede expresar.
- **Nunca conviven el valor en el manual y el valor en el código.** Esa duplicación *es* la deriva: el día que difieren, nadie sabe cuál manda.
- El cambio de estado lo registra el Leader en [`map.md`](../.claude/map.md) § *Evolución* y actualiza la fila de *Dónde vive el canon*.

## Cuándo se crea una página

Prueba de admisión, la misma que la de los *No negociables*: **si ninguna solution podría re-derivarla mal, no va.** Es descripción, y la descripción no es canon.

Una capa nace cuando una task la exigió, o cuando hay al menos un consumidor declarado en la tabla *Quién lo consume*. No se siembran capas "para cuando hagan falta".

## Extensión

**Cada página entra en una pantalla.** El manual crece por **capas nuevas**, nunca por prosa. Si una página no entra, casi siempre son dos capas mal fusionadas — no una página que necesita más espacio.

## Los tres tests (el gate del Leader los corre sobre cada página tocada)

1. **Aplicable sin gusto.** El Dev es ejecutor neutral: si para aplicar la regla hay que tener criterio estético, la página todavía no está escrita.
2. **Verificable en un diff.** Si la regla no puede atrapar una deriva concreta, sobra.
3. **No duplica el canon.** Cada valor vive una sola vez (ver arriba).

## Índice

| Capa | Estado | Página |
|---|---|---|
| _(ninguna todavía)_ | | |

> Este índice se refleja en [`map.md`](../.claude/map.md) § *Qué expone* — ahí es donde el resto del workspace lo lee.
