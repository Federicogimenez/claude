---
name: catalog
description: El catálogo de skills y librerías de terceros que el harness federicode tiene evaluadas — qué resuelve cada una, qué task la convoca, quién la usa y cómo se instala. Consultalo como Architect antes de buscar herramientas afuera, y siempre que una task pida narrativa animada, video, figuras explicativas, verificación en navegador, pruebas end-to-end, referencias de diseño, auditoría de UI o iconos animados.
---

# Catálogo de terceros

> Quién instala, cuándo y a qué nivel se enuncia en `format.md` § *Reglas transversales*. Acá vive **qué** hay, **qué task** lo convoca y **el comando**.

Antes de instalar, verificá en el repo oficial que la licencia y el comando sigan vigentes — este catálogo registra lo evaluado, no lo garantiza.

## Narrativa y explicación

| Skill | Resuelve | La convoca | Instalación |
|---|---|---|---|
| **anatomy** (MIT) | Una figura isométrica interactiva que explica un concepto técnico por partes | Explicar *cómo funciona algo que tiene piezas*: un sistema, un flujo de datos, un mecanismo | `npx skills add wheresryan22/anatomy -a claude-code` · Node ≥ 20.10. El modo 3D está en beta: no lo uses para entregables |
| **hyperframes** (HeyGen, Apache 2.0) | Una composición HTML/CSS/GSAP renderizada a MP4 determinístico | La pieza tiene que existir **fuera de la web**: video para redes, para una presentación o para embeber sin interacción | `npx skills add heygen-com/hyperframes --full-depth -a claude-code` · Node ≥ 22 + FFmpeg. Comandos: `/hyperframes`, `/hyperframes-cli`, `/gsap` |

### Elegir el medio

La decisión la toma el Architect, y la pregunta es qué tiene que hacer el espectador:

1. **¿Tiene que tocar para entender?** (cambiar un parámetro, ver qué pasa) → **experiencia web propia** con el enfoque `narrate`.
2. **¿Tiene que ver las partes de algo y cómo se conectan?** → **anatomy**. Como pieza sola o embebida en una experiencia.
3. **¿Tiene que verlo de corrido, sin interactuar, fuera del navegador?** → **hyperframes**. Si la pieza ya existe como experiencia web, se reusa su HTML: el video es un render de los mismos beats, no una pieza aparte.

En cualquier medio, la pieza parte del guion de beats del brief (enfoque `narrate`). El guion es diseño; el medio es plomería.

## Verificación

Playwright entra de dos formas. Las dos corren solo por criterio (`format.md`).

| Forma | Resuelve | La usa | Instalación |
|---|---|---|---|
| **Playwright MCP** (plugin oficial, envuelve `@playwright/mcp`) | Un navegador real que el agente maneja: navegar, interactuar, leer el DOM, sacar capturas | El Dev con `implement` | Viene con la siembra. No sumes también un MCP manual de Playwright: duplicaría el servidor |
| **Playwright Test** (framework) | Pruebas end-to-end que fallan si el comportamiento se rompe | El Dev con `test`, en el gate | `npm i -D @playwright/test` + `npx playwright install chromium` en la solution (no `npm init playwright`: es interactivo y un agente se cuelga). Es una dependencia del código, no una skill |

## Diseño de interfaz

| Skill | Resuelve | La convoca | Instalación |
|---|---|---|---|
| **taste-skill** (MIT) | Criterio anti-genérico para generar frontend: ritmo, tipografía, espaciado, movimiento. Tiene variantes por estética (`minimalist-skill`, `brutalist-skill`, `soft-skill`) | Una task de UI en la que el canon del `design-system/` **todavía no decide** algo | `npx skills add https://github.com/Leonxlnx/taste-skill --skill <variante> -a claude-code`. Elegí la variante que corresponda a la estética de la capa `estilo` |
| **image-to-code** (del repo taste-skill) | Diseñar primero la imagen, analizarla y recién después codificarla fiel | Landing o hero donde la fidelidad visual a una referencia es el criterio de éxito | `npx skills add leonxlnx/taste-skill --skill image-to-code -a claude-code` |
| **web-design-guidelines** (Vercel) | Auditar UI contra las Web Interface Guidelines: accesibilidad, interacción, tipografía. Baja las reglas vigentes en cada corrida | Cualquier task que toque UI: la corre el Leader en el gate | `npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines -a claude-code` |

> **El canon manda.** taste-skill e image-to-code completan lo que el `design-system/` no decide; **nunca** pisan un valor del canon. Si chocan, gana el canon, y el hueco que mostró la skill es una capa que falta en el `manual/`. Es trabajo del Architect, no del Dev.

## Referencias, no skills

| Recurso | Resuelve | Cómo entra |
|---|---|---|
| **awesome-design-md** (VoltAgent) | `DESIGN.md` de sistemas de marca conocidos, legibles por agentes | **No se instala.** Al derivar la capa `estilo`, el Architect trae **uno o dos** `DESIGN.md` cercanos a la dirección elegida a `design-system/resources/`, como material crudo. Es referencia, nunca canon |

## Librerías de runtime

Son dependencias del código de una solution, no skills del agente. Las decide una capa del `manual/`.

| Librería | Resuelve | La convoca |
|---|---|---|
| **morphicons** (MIT, ~7 KB, sin dependencias) | Morphing entre iconos de trazo (Lucide, Tabler, Heroicons, Iconoir o SVG propio) con física de resortes. Respeta `prefers-reduced-motion` | La capa `iconografia` declara iconos animados. Se usa para **cambios de estado** (menú↔cerrar, play↔pausa, check), no para decorar. Hay bindings para React, Vue, Svelte, React Native y web components |

## Evaluadas y descartadas

No se instalan. Si una task las pide, se re-evalúan desde el motivo:

- **ui-ux-pro-max** — es una base genérica de paletas y patrones que compite con el `design-system/` de cada workspace en vez de servirlo.
- **MCP de After Effects** — son proyectos comunitarios que necesitan AE abierto con un panel puente. hyperframes cubre el caso con HTML. Se re-evalúa solo si un cliente exige entregar el `.aep`.
- **OmniRoute** — el ahorro sale de desviar pedidos a otros modelos y de rotar cuentas. Rompe la previsibilidad del gate y choca con los términos de uso de la suscripción.
