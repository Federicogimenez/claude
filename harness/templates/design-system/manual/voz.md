# Voz — manual de design-system

> **Estado:** `canon`. La voz rara vez tiene implementación en código, así que esta página suele ser su fuente de verdad. Excepción: si existe un archivo central de copy/i18n, ese archivo manda para los **textos** y esta página queda como la **regla** que los gobierna (`spec`). La regla de estados se define en [README.md](README.md) § *Canon o spec*.
> **Consumen:** <toda solution con texto de cara al usuario>

Viene sembrada porque su forma no es una tabla de tokens. Si el trabajo no tiene texto de cara al usuario, borrala.

## De dónde sale
> La voz **no se inventa acá.** Se deriva del posicionamiento y la audiencia declarados en [`business/.claude/CLAUDE.md`](../../business/.claude/CLAUDE.md) § *Decisiones de negocio que condicionan el diseño*; esta página los traduce a reglas aplicables. Si al escribirla no hay de dónde derivar, el hueco está en `business/`, no acá — se llena primero allá.

Es la misma cadena que el resto del lenguaje: **`business/` decide, `design-system/` lo hace aplicable, las solutions lo citan.**

## Las decisiones

| Decisión | Definición | Por qué |
|---|---|---|
| Persona gramatical | <vos · tú · usted · impersonal> | |
| Registro | <cercano · neutro · formal — y explícitamente qué NO es> | |
| Quién habla | <el producto · el equipo · nadie — "Guardamos tus cambios" vs. "Cambios guardados"> | |
| Longitud por defecto | <p. ej. botón ≤ 2 palabras, título ≤ 6, error ≤ 1 oración> | |

## Glosario del producto
> Un concepto, un nombre. Que la misma cosa se llame distinto en dos solutions es la deriva de marca más barata de evitar y la más cara de arreglar después.

| Concepto | Se dice | No se dice |
|---|---|---|
| <concepto> | <término> | <sinónimos que aparecen solos> |

## Cómo hablan los momentos
> Los cuatro donde toda solution improvisa si no está escrito.

| Momento | Regla | Ejemplo |
|---|---|---|
| Error | <¿se disculpa? ¿culpa al usuario? ¿ofrece salida?> | |
| Estado vacío | <¿explica o invita a la acción?> | |
| Confirmación destructiva | <¿nombra la consecuencia exacta e irreversible?> | |
| Éxito | <¿celebra o confirma seco?> | |

## Qué no

- <p. ej. sin signos de exclamación en microcopy funcional · sin "Oops" · sin humor en flujos de dinero · sin culpar al usuario>

## Qué decide cada solution (y no esta página)
> La **densidad y la intención** del texto son de cada solution: un backoffice es telegráfico, una landing persuade. Se declaran en su `fundamentals/` **citando** esta página, nunca re-declarando la voz. Si una solution necesita contradecir algo de acá, no lo hace por su cuenta: es una divergencia y va a [map.md](../.claude/map.md) § *Evolución*, con motivo.

## Origen
Task `<NNNN-slug>` · material de [`../resources/`](../resources/): <qué lo fundó>.
