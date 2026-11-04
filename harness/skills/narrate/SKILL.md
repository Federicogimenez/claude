---
name: narrate
description: El enfoque del Dev para construir piezas narrativas o explicativas animadas — experiencias web que cuentan un proceso por beats, figuras interactivas o composiciones para video. Convierte el guion de beats del brief en estados direccionables. Úsala al implementar cualquier pieza que explique algo en secuencia.
---

# Skill: narrate

> Disciplina de ejecución → persona del Dev (`agents/dev.md` del harness). Acá solo el enfoque propio de narrar. El medio (web propia, anatomy, hyperframes) y el guion de beats vienen decididos en el handoff.

## El enfoque

Una pieza narrativa es una **secuencia de beats**. Cada beat es un estado con nombre que muestra **una sola idea**. El movimiento entre beats es la explicación: muestra causa → efecto.

- **Un beat = un estado direccionable.** Cada beat del guion tiene un id estable y se puede llegar a él directo, por scroll, por URL o por la posición en el timeline, sin reproducir lo anterior. Las animaciones van en un timeline **seekable** (pausado y posicionable por beat), nunca encadenadas a eventos que solo corren hacia adelante. Así la misma pieza sirve a la web, al render de hyperframes y a las pruebas.
- **El movimiento explica, no decora.** Cada animación responde a qué cambió entre un beat y el siguiente. Si un movimiento no corresponde a un cambio del guion, sobra.
- **Respetá la intensidad de movimiento** que el handoff cita de la capa `movimiento`. No la subas por tu cuenta.
- **Reduced motion es un camino completo.** Con `prefers-reduced-motion`, cada beat se muestra como estado final, sin transición. La historia se tiene que seguir entendiendo entera.
- **Construí el beat estático primero y la transición después.** Si el estado final de un beat no se lee solo, ninguna animación lo arregla.
- **Que se pueda probar:** exponé el beat activo de forma observable (un atributo `data-beat` o equivalente), para que el enfoque `test` verifique "en el beat N se ve X" sin comparar píxeles.
- **Si el guion no alcanza** (un beat sin estado final claro o una transición sin causa), escalá al Architect. No inventes la narrativa.
