# business — Autoridad de integración

> Se carga al trabajar dentro de `business/`. Hereda el formato de trabajo global del harness federicode (`/harness:format`) y los particulares del workspace ([../../CLAUDE.md](../../CLAUDE.md)). Es un **aligner**: no tiene objetivo propio, existe para que las solutions no re-deriven lo transversal por su cuenta. Este archivo es la **autoridad de integración** del workspace: el norte del negocio, qué expone/consume cada sub-proyecto y qué decisiones de negocio condicionan el diseño.

## Qué es <negocio>
<Qué es el producto/negocio y para quién — 2-3 frases.>

## Etapa actual (<fecha>)
- <La necesidad de *ahora*: qué se persigue.>
- <Qué explícitamente todavía no.>

## Decisiones de negocio que condicionan el diseño
- <Decisión que restringe el diseño — ej. modelo de cobro, posicionamiento legal, canal de coordinación con el usuario.>
- **Posicionamiento y audiencia:** <cómo se para el negocio frente a quién.> Es el **origen de la voz**: `design-system/manual/voz.md` lo traduce a reglas aplicables (persona, registro, glosario) y no lo inventa. Si esa página no tiene de dónde derivar, el hueco está acá.

## No negociables
> Enunciados verificables, en imperativo, con cómo se comprueban. MÁXIMO 4. Prueba de admisión: si el enunciado no cambiaría una decisión del Architect ni atraparía una deriva en el gate del Leader, no va acá — es descripción, y la descripción vive en las otras secciones.

- **<Enunciado en imperativo>.** Se verifica: `<cómo se comprueba en un diff o en el producto>`.

Los lee el Architect al diseñar y el Leader en el gate. El Dev nunca los lee: le llegan citados en el handoff.

## Integración (quién expone qué)

| Proyecto | Rol en el negocio | Expone | Consume |
|---|---|---|---|
| <solution/> | <rol de esta solution en el negocio> | <qué le da a los demás> | <qué necesita de los demás> |

Esta tabla es también la lista de a quién nuclea `business/`: cuando este archivo cambia, el gate del Leader revisa el `fundamentals/` de cada fila.

## Estado
🌱 Bootstrap.

## Memoria del proyecto — dos cadencias
Como no tiene código, no tiene grafo: quedan las dos capas de prosa.

| Capa | Cadencia | Qué guarda |
|---|---|---|
| [map.md](map.md) | curada por el **Leader**, por commit | Mapa de integración detallado + § *Evolución* (principios, decisiones, ideas parqueadas) |
| [tasks/](tasks/) | **inmutable**, por cambio | La crónica del *por qué* |

> Su intake es la carpeta `resources/` (material crudo que carga el humano; el Architect lo procesa en las definiciones de este archivo). Las ideas descartadas o pospuestas quedan en `map.md` (§ *Evolución*) con motivo.
