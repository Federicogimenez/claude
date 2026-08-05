# Cómo se crea una task

> **Este archivo no se copia.** Documenta el template: al crear una task copiás `brief.md`, `tasks.md` y `doc/`, y este README se queda acá.
>
> Es la **definición única** de cómo nace, avanza y se cierra una task. `format.md` la referencia y no la repite; las personas del harness operan desde acá.

## Quién la crea y cuándo

La crea el **Architect**, después de entender la raíz del problema y **antes** de delegar al Dev.

Una task **ínfima** (instrucción directa, sin juicio de diseño nuevo) puede no llevar carpeta. Ahí el reporte final del Dev es el único registro, y el Architect **se lo dice explícitamente** en el handoff — si no, el Dev busca una carpeta que no existe.

## Dónde vive y cómo se numera

```
<solution>/.claude/tasks/NNNN-slug/
```

- Va en la **solution** que toca — nunca en la raíz del workspace. `business/` y `design-system/` también llevan las suyas.
- **`NNNN`**: secuencial de cuatro dígitos **por solution** (`0001-`, `0002-`, …). El siguiente número es el mayor existente + 1. Los números no se reciclan ni se reordenan.
- **`slug`**: kebab-case corto, en el idioma del proyecto.
- Un cambio que toca **varias solutions** es **una task por solution** — no una task raíz. Cada una lleva su propio número y declara el vínculo en su `brief.md` (*Vínculo cross-project*).

## Qué se copia y quién escribe cada archivo

| Archivo | Lo escribe | Cuándo |
|---|---|---|
| `brief.md` | **Architect** | Antes de delegar — es la raíz de la task |
| `tasks.md` | **Architect** lo arma · Architect y Dev lo **marcan** | Durante la ejecución |
| `doc/README.md` | **Leader** | En el gate, verificando la implementación contra el brief y las pruebas |

La `doc/` **no la escribe el loop**: durante la ejecución solo se marca `tasks.md`. La escribe el Leader al final, para documentar lo que quedó construido y no lo que se pensaba construir. Es entregable y **se versiona** — lo desechable va a `.scratch/`.

## El ciclo de estados

Vive en la sección *Estado* de `tasks.md`:

```
borrador → en progreso → en QA (humano) → ajustes → en revisión (leader) → cerrada
```

| Transición | Quién la mueve |
|---|---|
| `borrador` → `en progreso` | **Architect**, al delegar al Dev |
| `en progreso` → `en QA (humano)` | **Architect**, al surfacear el resultado y parar |
| `en QA` ↔ `ajustes` | **Architect**, en cada vuelta de corrección por delta al mismo Dev |
| → `en revisión (leader)` | **el humano**, al disparar el gate |
| `en revisión` → `cerrada` | **Leader**, después de escribir la `doc/` y commitear |

## Cerrada es inmutable

`tasks/` es la crónica del *por qué* por cambio: una task cerrada **no se reescribe ni se renumera**. Si algo cambió después, es una task nueva.

Lo que sí evoluciona — principios, deuda, ideas parqueadas — no vive acá: vive en `map.md` § *Evolución*, que el Leader poda y fusiona en cada commit.
