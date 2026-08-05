# <Capa> — manual de design-system

> **Estado:** `canon` (todavía sin implementación — esta página es la fuente de verdad) · `spec` (implementado en `<ruta>` — ese código manda; acá solo la regla). La regla se define en [README.md](README.md) § *Canon o spec*.
> **Consumen:** <solutions de la tabla *Quién lo consume* de [../.claude/CLAUDE.md](../.claude/CLAUDE.md)>

## La decisión
<Una frase: qué se resolvió y contra qué alternativa. El porqué largo vive en el `brief.md` de la task, no acá.>

## Los valores
> En `canon`, la columna *Valor* trae el valor literal. En `spec`, se reemplaza por el puntero al token real — el valor **no se repite acá nunca**.

| Token | Valor | Qué resuelve |
|---|---|---|
| <nombre> | <literal · o `→ src/tokens/<archivo>`> | <cuándo se usa este y no otro> |

## Cuándo se aplica cada uno
> Reglas decidibles **sin criterio estético**: el Dev las aplica sin diseñar. Si para elegir hay que tener gusto, la regla todavía no está escrita.

- <regla de aplicación>

## Qué no
> Los anti-usos. Es la sección que el Leader puede verificar en un diff.

- <lo que no se hace — y qué se hace en su lugar>

## Origen
Task `<NNNN-slug>` · material de [`../resources/`](../resources/): <qué lo fundó> · divergencias con `resources/` registradas en [map.md](../.claude/map.md) § *Evolución*.
