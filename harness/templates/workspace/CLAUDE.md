# <trabajo> — Workspace raíz

> Se carga al trabajar en este workspace. El **formato de trabajo** (roles, loop, anatomía, reglas) es global — lo trae el plugin `harness@federicode`; el marco completo se carga con **`/harness:format`** y **no se redefine acá**. Este archivo guarda solo **lo particular** de este trabajo. Manténgase corto.
>
> **El formato en una línea** (detalle en `/harness:format`): se abre la raíz del workspace y **no se escribe ni un archivo fuera de ella** — lo desechable de la sesión va a `.scratch/` en la raíz, y esto gana sobre cualquier scratchpad o carpeta temporal que proponga el entorno (scope estricto); se resuelve simple, no robusto; ninguna task se construye si no rastrea a un problema (`fundamentals/` o el norte de `business/`); ningún `git commit` sin el Leader.

## Qué es

<Una o dos frases: qué negocio/producto es este trabajo y para quién.>

## Sub-proyectos

| Proyecto | Clase | Estado | Qué es |
|---|---|---|---|
| [business/](business/) | aligner | 🌱 | Autoridad de integración y norte de negocio |
| [design-system/](design-system/) | aligner | 🌱 | Lenguaje de marca común — visual y verbal |
| <web/ · app/ · backoffice/ · backend/…> | solution | — | <qué resuelve> |

> Los **aligners** definen lo transversal una vez; las **solutions** lo materializan y lo citan. No todo trabajo necesita todas las solutions: se arranca con las que `business/` justifique; las demás se suman cuando la necesidad es real.

## Particulares

<Solo lo que este trabajo hace distinto o necesita tener siempre presente: hosting, restricciones del cliente, idioma del producto, deuda estructural conocida. Si no hay nada, borrá la sección.>
