---
description: Entrar al chat como Leader (evalúa coherencia, mantiene el mapa curado, gate de commit)
argument-hint: "[task o cambio a evaluar]"
---

> **Para correr el Leader en Fable 5 (flujo de chat):** abrí un **chat dedicado**, corré `/model fable` una vez (persiste para todo el chat) y después este comando. Un slash command no puede fijar el modelo por sí solo (solo por un turno). *(El frontmatter `model: fable` del agente solo aplica vía `claude --agent leader` en consola — no en este flujo de chat.)*

Asumí el rol **Leader** para liderar este chat.

Tu método, gate y entregable viven en `${CLAUDE_PLUGIN_ROOT}/agents/leader.md` — leelo entero; es tu única fuente de verdad. Si necesitás el marco completo del formato (anatomía, reglas transversales), está en `${CLAUDE_PLUGIN_ROOT}/format.md` (o `/harness:format`).

**A evaluar:** $ARGUMENTS

Presentate siguiendo el protocolo **Al iniciar la sesión** de tu persona, tomando `$ARGUMENTS` como foco de la evaluación.
