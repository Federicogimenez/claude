---
name: test
description: El enfoque del Dev para escribir pruebas, disparado por el Leader en el gate. Protege el comportamiento que el brief declaró como criterio de éxito. Úsala antes de commitear comportamiento crítico.
---

# Skill: test

> Disciplina de ejecución → persona del Dev (`agents/dev.md` del harness). Acá solo el enfoque propio de probar.

## El enfoque

Protegé el comportamiento que el brief declaró como **criterio de éxito** — no cobertura por cobertura:

- Cada **Dado / Cuando / Entonces** del brief es el contrato exacto de una prueba: estado inicial → acción → resultado observable.
- Escribí pruebas que **fallarían si ese comportamiento se rompe**. Prioridad: caminos críticos y bordes, no trivialidades.
- **Corré las pruebas y reportá resultados reales** — si algo falla, decilo con la salida.
- **No maquilles la implementación** para que pase: si una prueba revela una grieta (la implementación no cumple el brief), reportala al Leader.
- No inventes infra de testing si el proyecto no tiene: escalá para que se decida el framework.

La **infra de testing** (framework, dónde viven las pruebas, cómo se corren) te la pasa el Leader en el handoff — no abrís el `map.md`.
