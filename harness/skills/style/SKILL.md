---
name: style
description: El árbol de decisión de estilo visual del harness federicode — cómo pasar del posicionamiento de business/ a una dirección estética (minimalismo, brutalismo, futurismo, glassmorfismo, liquid glass, claymorfismo, maximalismo), una composición (bento, scroll narrativo, grilla densa) y una profundidad (plana o espacial). Usala como Architect al derivar la capa `estilo` de design-system/, o cuando una propuesta de diseño no tiene dirección decidida.
---

# Árbol de estilo

> Lo usa el **Architect**. La salida no es una opinión: es la capa **`estilo`** del `design-system/manual/`, que se escribe con la misma plantilla y los mismos tres tests que cualquier otra capa (`manual/README.md`).

## Tres decisiones, no una

Los nombres que circulan como "estilos" mezclan tres ejes. Si se confunden, se elige "bento" contra "glass" como si fueran alternativas, y no lo son.

| Eje | Qué decide | Dónde vive | Cuántas |
|---|---|---|---|
| **1. Estética** | Cómo se ven las superficies: color, sombra, borde, textura, tipografía | La capa `estilo` del `design-system/` | **Una por marca.** Mezclar dos es deriva |
| **2. Composición** | Cómo se ordena el contenido en la pantalla | El `brief.md` de cada pieza | Una por pieza, dentro de la estética |
| **3. Profundidad** | Si la interfaz usa el eje Z (capas, perspectiva, 3D) | El `brief.md` de cada pieza | Una por pieza |

## 1. Estética — se deriva de `business/`

La pregunta raíz es **qué tiene que sentir la audiencia en el primer segundo**. Se toma del posicionamiento declarado en `business/`; si no está declarado, la estética no se puede elegir todavía, y eso se vuelve el primer entregable.

| Si tiene que sentir… | Estética | Rasgos verificables (van a la capa) | Riesgo |
|---|---|---|---|
| **Confianza, precisión, calma** | **Minimalismo** | Paleta de 1 acento + neutros. Jerarquía solo por tipografía y espacio. Sin sombras decorativas. Mucho aire | Caer en lo genérico: el acento y la tipografía cargan toda la identidad |
| **Actitud, honestidad, diferencia** | **Brutalismo** (y neo-brutalismo) | Estructura a la vista, bordes duros y gruesos, contraste máximo, tipografía de sistema o mono. En la variante neo: sombras duras desplazadas, sin blur | Que parezca roto. Las reglas tienen que ser estrictas justamente porque se ve crudo |
| **Asombro, innovación, tecnología** | **Futurismo** | Fondo oscuro, acentos luminosos o glow, grillas técnicas, tipografía geométrica o mono, motion de "sistema vivo" | Ser decorativo sin explicar. El glow pierde contraste |
| **Sofisticación en capas, contexto que se mantiene** | **Glassmorfismo** | Paneles translúcidos con `backdrop-filter: blur`, borde claro de 1px, fondo con color debajo para que el vidrio se lea | Contraste de texto insuficiente. No sirve para datos densos |
| **Lo mismo, pero vivo y premium** | **Liquid glass** | El vidrio refracta y reacciona al movimiento: lente dinámica, brillo especular, deformación al interactuar | **Performance y soporte.** La refracción real con filtros SVG sobre el backdrop es pesada y desigual entre navegadores. Exige un fallback a glass plano |
| **Cercanía, juego, confianza amable** | **Claymorfismo** | Formas infladas, radios grandes, doble sombra (externa suave + interna clara), pasteles saturados | Lo infantil. No escala a densidad de información |
| **Energía, abundancia, cultura** | **Maximalismo** | Color intenso y múltiple, mezcla tipográfica, capas e ilustración, el contenido como collage | Ruido. Exige una regla de jerarquía muy firme para que algo se lea primero |

**Filtros que descartan antes de elegir.** Si aplica alguno, la estética candidata se cae:

- **Accesibilidad crítica** (público amplio, salud, gobierno, finanzas personales) → descartá glass, liquid glass y maximalismo para superficies con texto.
- **Datos densos** (tablas, dashboards) → descartá glass y clay en esas superficies.
- **Dispositivos modestos o performance como criterio de éxito** → descartá liquid glass, blur pesado y 3D.

**Combinación permitida:** una estética **base** y, como mucho, un **acento** acotado a un componente. Por ejemplo, base minimalista con glass solo en el overlay de navegación. El acento se declara en la capa con el componente exacto; sin eso, es deriva.

## 2. Composición — por pieza

| El contenido es… | Composición |
|---|---|
| **Varias cosas paralelas sin orden** (features, servicios, proyectos de portfolio) | **Bento grid**: celdas de tamaños distintos, donde el tamaño codifica la importancia |
| **Un proceso o una historia en orden** | **Scroll narrativo por beats** (enfoque `narrate`). Nunca bento: la grilla rompe la secuencia |
| **Datos para comparar** | **Grilla densa**, alineación estricta |
| **Una sola acción** | **Foco único**: un mensaje y un CTA |

## 3. Profundidad — por pieza

- **Plana** por default. La profundidad se paga en performance y en complejidad, así que se justifica.
- **UI espacial** (capas en Z, perspectiva, parallax con sentido, 3D) cuando **la idea misma es espacial**: un sistema con partes, un flujo que atraviesa capas o un producto físico. En ese caso, la figura explicativa sale de **anatomy** (isométrica) antes que de un 3D propio (ver `harness:catalog`).
- La profundidad **nunca** es el único canal de una información: con `prefers-reduced-motion` o sin WebGL, la pieza se tiene que seguir entendiendo.

## Movimiento e iconos

La estética fija la **intensidad** del movimiento. Cuánta exactamente se escribe en la capa `movimiento`:

- **Baja**: minimalismo, brutalismo. Transiciones cortas y funcionales.
- **Media**: glass, clay, maximalismo.
- **Alta**: futurismo, liquid glass.

Los **iconos animados** (morphicons, ver `harness:catalog`) se justifican en cualquier estética **para cambios de estado**. Como decoración, solo en intensidad alta.

## Cómo se propone

1. Pasá los filtros y quedate con **dos candidatas como máximo**.
2. Mostralas: la misma pantalla en las dos direcciones, en una página de comparación en `.scratch/`, con los rasgos verificables a la vista. El humano elige viendo, no leyendo.
3. Recomendá una, con el motivo atado al posicionamiento de `business/`.
4. Con la elección, traé uno o dos `DESIGN.md` de referencia a `design-system/resources/` (ver `harness:catalog`) y escribí la capa `estilo`:
   - la estética base (y el acento, si hay, con su componente);
   - sus rasgos verificables como reglas aplicables sin gusto;
   - los filtros que descartaron a las demás, como anti-usos.
