# Tasks — <NNNN-slug>

> Checklist de ejecución derivado del brief. El Architect lo arma; Architect y Dev lo van marcando durante el loop. Quién mueve cada estado, en [`templates/task/README.md`](../README.md) del harness.

## Objetivos
- [ ] <objetivo derivado directamente del criterio de éxito del brief> (CE-1)

## Ejecución
- [ ] <paso concreto>
- [ ] <paso concreto>

## QA visual (humano)
- [ ] Revisión visual del humano OK (o ajustes pedidos y aplicados por delta al mismo Dev)

## Gate del Leader (lo dispara el humano, antes de commit)
- [ ] Coherencia raíz→implementación verificada (proporcional al peso)
- [ ] Cada CE verificado o justificado; sin `[AMBIGUO]` abierto si la task es completa
- [ ] Pruebas del comportamiento crítico, en verde
- [ ] `doc/` escrita contra lo que quedó construido (qué se construyó · cómo funciona · veredicto · notas)
- [ ] `map.md` actualizado (capa curada + *Evolución* si aplica)
- [ ] Commit hecho y task movida a `cerrada`

## Estado
`borrador` → `en progreso` → `en QA (humano)` → `ajustes` → `en revisión (leader)` → `cerrada`

_Actual:_ `borrador`
