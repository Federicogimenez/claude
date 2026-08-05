# Brief — <NNNN-slug>

> Lo escribe el **Architect** antes de tocar código. Es la raíz: si esto no está claro, no se implementa todavía. Cómo se numera y se cierra la task, en [`templates/task/README.md`](README.md) del harness.

## Peso
`liviana` | `completa` — **liviana** salta secciones: llená solo *El problema*, *El enfoque elegido*, *Scope de archivos*, *Supuestos y ambigüedad* (breve) y *Criterio de éxito*. **Completa** llena todo. (El Leader calibra su gate según esto.)

## El problema
<¿Qué dolor o fricción existe? ¿Qué proceso se quiere agilizar?>

## La raíz
<¿Por qué existe ese problema? El problema de fondo, no el síntoma.>

## La necesidad de esta etapa
<¿Qué necesita el proyecto resolver *ahora*? No lo que necesitará después.>

## El enfoque elegido
<La solución propuesta, en términos de cómo responde a la necesidad de arriba.>

## La skill del Dev (el enfoque)
<Qué enfoque carga el Dev para esta task (`implement`, `test`, una instalada, o uno nuevo a definir). La skill es solo el enfoque; la instrucción condensada + punteros se la pasás en el handoff al delegar.>

## Vínculo cross-project
> Solo si esta task es parte de un cambio que toca varios proyectos — que se resuelve con **una task por proyecto**, nunca con una task raíz. Si no aplica, "ninguno".
- **Tasks hermanas:** <ej. `backoffice/0005-endpoint-pedidos`, `web/0003-vidriera`>
- **Dependencia:** <qué expone/consume esta task respecto de las hermanas, y la dirección (quién va primero)>
- **Integración:** <el contrato/decisión que cruza proyectos — debe quedar registrado en `business/.claude/CLAUDE.md`>

## Scope de archivos
> Las fronteras que el Dev respeta. Lo no listado en CREAR/MODIFICAR es NO TOCAR por defecto.
```
CREAR:
  <archivos nuevos>
MODIFICAR:
  <archivos existentes que se pueden tocar>
NO TOCAR:
  <archivos sensibles fuera del scope, si hace falta nombrarlos>
```

## Alternativas descartadas
<Qué otros caminos se consideraron y por qué no. (Esto es lo que el Leader evalúa para juzgar coherencia.)>

## Tecnología / investigación
<Hallazgos relevantes del estado actual de la tecnología, si la decisión dependió de ello.>

## Supuestos y ambigüedad
<Qué asumiste EN VEZ DE preguntar, explícito. Usá el marcador `[AMBIGUO: <qué falta decidir>]` inline en cualquier sección donde algo haya quedado sin resolver.>

> **Qué cuenta como ambiguo:** dos lecturas razonables de este brief producen dos entregables distintos y el brief no da con qué elegir. Lo que el Dev resuelve espejando una convención existente **no** es ambiguo — es un puntero que falta en el handoff.
>
> En task **completa**, un `[AMBIGUO]` sin resolver es **bloqueante** en el gate del Leader. En **liviana**, se registra y no bloquea.

## Criterio de éxito
> En formato verificable **Dado / Cuando / Entonces** — cada criterio con ID, un contrato exacto: la skill `test` escribe pruebas contra esto y el Leader lo usa en el gate.

- **CE-1** — Dado <estado inicial>, cuando <acción o trigger>, entonces <resultado observable>.
- <tantos como comportamientos protege la task — pocos y de fondo, no trivialidades>.
