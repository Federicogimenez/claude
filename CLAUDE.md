# claude — el harness federicode

Este repo **es** el harness federicode: la fuente de verdad del formato de trabajo que consumen todos los demás workspaces vía plugin. Acá el harness **se edita, no se consume**.

## Cómo se abre

**Con `claude --plugin-dir ./harness`.** Carga el working tree de esta sesión, así que el rol que corre es el que estás editando. Sin ese flag, la sesión corre la copia del cache — congelada en un commit anterior — y vas a editar un archivo distinto del que te está gobernando. `/reload-plugins` recarga sin reiniciar tras cada cambio. Detalle completo en [README.md](README.md), sección *Mantenimiento*.

## Cómo se gobierna (distinto de un workspace)

- **No lleva anatomía de `tasks/`.** Se relee entero cada sesión, así que una decisión se registra como **propiedad del texto vigente**, no como historia sobre él. El porqué crudo va al mensaje de commit — `git log` es su crónica.
- **El gate no es proporcional al peso.** Es un barrido de coherencia sobre el texto completo: tocar `format.md` obliga a revisar las tres personas (`agents/`), los cuatro comandos (`commands/`), el `README.md` y los templates.

## Dónde vive cada definición (la regla que evita la deriva)

Cada regla tiene **un dueño y tres apariciones como máximo**:

| Dónde | Qué va | Extensión |
|---|---|---|
| `format.md` | **El enunciado.** La regla completa, una sola vez — es el spec. | Lo que haga falta |
| La persona del rol que la **ejecuta** (`agents/`) | **La operación.** Cómo se aplica desde ese asiento, y nada más. | Una línea o un paso |
| El template, **en el punto de uso** | **El recordatorio.** Que aparezca donde se está escribiendo. | Una línea |

**No hay cuarta copia.** Si una regla aparece en un comando, en el README y además en dos personas, ya divergió o va a divergir. Los comandos (`commands/`) no definen nada: fijan el rol, pasan `$ARGUMENTS` y delegan a la persona. El `README.md` es la puerta de entrada humana (qué es, instalar, usar, mantener, forkear) y **no reescribe el spec**: el orden de lectura es README → `format.md`.

Una excepción explícita: el bloque **El sistema en el que estás** es **idéntico en las tres personas** a propósito — es lo que hace que un rol conecte el harness entero sin cargar `format.md`. Se toca en los tres a la vez o en ninguno.

## El formato completo

Vive en [`harness/format.md`](harness/format.md) (o `/harness:format` desde una sesión con el plugin cargado).

## Scope

Se escribe solo bajo esta raíz. Lo desechable (borradores, salidas intermedias) va a `.scratch/`, gitignorado.
