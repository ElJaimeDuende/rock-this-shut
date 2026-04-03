---
name: rock-this-shut
description: Transposes the visual language of a reference image or GIF to a new use case. Produces a technical spec and approved mock ready for the dev workflow. Phases: DECODE → ADAPT → SPECIFY → MOCK.
---

# Rock This Shut

## Overview

## Phase 1 — DECODE

**Trigger:** User invokes the skill. Claude immediately asks:
> "Comparte la imagen o GIF de referencia para comenzar."

**Claude does — in this order:**

1. Analyzes the image visually. Produces a numbered component list using letter labels (A, B, C...). For each component include:
   - Label and name (e.g., "A — Título parpadeante")
   - Visual description: shape, color, typography, size, position relative to the whole
   - Animation (if any): what changes, how, at what speed
   - Apparent function within the composition

   Example output format:
   ```
   A — Título en mayúsculas
      Color: verde (#00FF41), fondo negro
      Tipografía: monospace, uppercase, ~18px
      Animación: parpadeo lento (~1s on/off)
      Posición: top-center
      Función aparente: identificador de la pantalla

   B — Órbita circular
      Color: línea verde punteada sobre fondo negro
      Tamaño: ~60% del ancho de la pantalla
      Animación: ninguna (estática)
      Posición: center
      Función aparente: trayectoria de referencia
   ```

2. After presenting the component list, says:
   > "Revisá esta lista. Podés:
   > - Agregar componentes que no identifiqué (decime qué ves que falta)
   > - Editar el nombre, descripción o interpretación de cualquier componente
   > - Eliminar los que no te hagan sentido
   > ¿Hay algo que cambiar?"

3. Applies all edits the user requests. Re-presents the updated list after each batch of changes.

4. When the user confirms the component list, opens the holistic conversation:
   > "Ahora hablemos del conjunto. ¿Qué es esta pantalla? ¿Para qué se usa? ¿En qué contexto existe — software, industria, época, propósito operacional? Contame lo que sepas o intuyas."

5. Engages in free conversation. Contributes its own interpretation. Synthesizes a shared understanding paragraph:
   > "Entonces, lo que tenemos es: [síntesis del conjunto: qué es, para qué sirve, qué comunica, qué mood evoca]."

6. **GATE:** Asks explicitly:
   > "¿Este entendimiento del conjunto refleja bien lo que ves? Confirmá para continuar a la siguiente fase."

   Does NOT advance to ADAPT until the user confirms.

## Phase 2 — ADAPT

**Trigger:** User confirms DECODE understanding. Claude asks:
> "¿Cuál es el nuevo uso que querés darle a este objeto visual? Describí qué querés hacer o mostrar."

**Claude does:**

1. After receiving the new use case, works through the component list **one component at a time**. For each component:

   a. States the original component and its function:
      > "Componente B — Órbita circular. En el original, es la trayectoria de referencia que define el espacio donde se mueven los objetos."

   b. Proposes a mapping to the new use:
      > "Para tu caso de seguimiento de equipo, esta órbita podría representar [propuesta concreta]."

   c. Asks:
      > "¿Te parece bien este mapeo, o preferís [alternativa A] / [alternativa B]?"

   d. If the component doesn't map cleanly, says so explicitly:
      > "Este componente no tiene un equivalente claro en tu nuevo uso. Opciones: (1) eliminarlo, (2) reemplazarlo por [propuesta], (3) adaptarlo a [propuesta alternativa]. ¿Cuál preferís?"

   e. Records the confirmed mapping before moving to the next component.

2. After all components are mapped, presents the complete mapping summary:
   ```
   MAPEO COMPLETO:
   A — Título parpadeante → Nombre del dashboard / título del tracker
   B — Órbita circular → Ciclo o período de seguimiento (sprint, semana, mes)
   C — 4 puntos rotativos → Cada miembro del equipo (4 líderes)
   D — Sets de números → KPIs resumidos por líder (ej: tareas abiertas, % completado)
   ```

3. **GATE:** Asks:
   > "¿Este mapeo completo refleja lo que querés construir? Confirmá para pasar a la especificación técnica."

   Does NOT advance to SPECIFY until the user confirms.

## Phase 3 — SPECIFY

**Trigger:** User confirms ADAPT mapping. Claude begins the technical specification.

**Claude does:**

1. Writes the technical specification covering:
   - Component inventory: each component with its visual properties and new semantic role
   - Animation specs: timing, easing, keyframe descriptions for animated components
   - Layout: container dimensions, positioning system (CSS grid/flex/absolute), responsive behavior if applicable
   - Color system: exact hex values extracted from reference, mapped to semantic roles
   - Typography: font family, sizes, weights, letter-spacing, for each text component

2. Decides which tools/libraries to use. Evaluates and selects from:
   - Vanilla HTML/CSS/JS (default for standalone components)
   - CSS animations vs JavaScript animation libraries (GSAP, Anime.js, etc.)
   - Canvas/SVG for complex orbital or geometric components
   - Framework-specific (React, Vue) only if the target project uses one

   If uncertain about a library's current API or availability, performs a web search before deciding.

3. Presents decisions with explicit justification:
   ```
   DECISIONES TÉCNICAS:

   Animaciones: CSS animations (keyframes)
   Razón: Las animaciones son simples (parpadeo, rotación continua). No justifica
   una librería externa. CSS animations son más performantes para este caso.

   Geometría orbital: SVG
   Razón: La órbita es un path circular. SVG permite animación precisa de posición
   sobre un path sin cálculos manuales de trigonometría.

   Stack base: HTML + CSS + Vanilla JS
   Razón: Componente standalone, sin framework requerido. Fácil de integrar.
   ```

4. **GATE:** Asks:
   > "¿Las decisiones técnicas tienen sentido para tu proyecto? ¿Hay algo que cambiar antes de pasar al mock?"

   Does NOT advance to MOCK until the user confirms.

## Phase 4 — MOCK

**Trigger:** User confirms SPECIFY decisions. Claude generates the visual mock.

**Claude does:**

1. Determines mock format based on environment:
   - If Claude Preview MCP tools are available (`mcp__Claude_Preview__*`): generates static HTML file and opens it in the browser preview
   - If Claude in Chrome MCP tools are available (`mcp__Claude_in_Chrome__*`): generates HTML and opens via browser
   - If no browser tools available: generates the complete HTML/CSS code as a code block and describes each visual element explicitly

2. Generates the mock as a **single self-contained HTML file** with inline CSS and JS. No external dependencies. The mock must:
   - Render all components identified in DECODE
   - Apply the semantic mapping from ADAPT
   - Use the colors, typography, and animations from SPECIFY
   - Be visually close enough to the reference that the user can evaluate the transposition

3. After showing/presenting the mock, asks about each area:
   > "¿Cómo se ve? Podés pedirme ajustes por componente. Ejemplo: 'el punto C se mueve muy rápido' o 'el color del título no es el que tenía en mente'."

4. Applies adjustments iteratively. Regenerates the mock after each batch of changes.

5. **GATE + HANDOFF:** When the user approves the mock, says:
   > "Mock aprobado. Voy a generar los archivos de salida."

   Then writes the two output files (see Output Templates section).

   Then delivers the handoff message:
   > "Spec y plan listos en `docs/rock-this-shut/`. El siguiente paso es el dev workflow: usa `tdd-workflow`, `subagent-driven-development`, o el proceso que uses normalmente para desarrollo."

## Behavior Rules

**This skill NEVER:**
- Advances to the next phase without explicit user confirmation at the gate
- Writes functional code (HTML mock for visualization only — all production code is written by the dev workflow)
- Assumes the new use case — always asks explicitly at the start of ADAPT
- Presents multiple ADAPT components at once — always one at a time
- Skips a phase even if the path seems obvious

**Animated GIF handling:**
Extract 3-5 key frames that show distinct visual states. List them as "Frame 1", "Frame 2", etc. Treat each unique animation state as a component property, not a separate component. Example: "Component C — Rotating points: 4 points on the orbit path. Animation: continuous clockwise rotation, ~8s full cycle."

**Ambiguous reference style:**
If the image has more than one plausible semantic interpretation, present 2-3 options before starting the holistic conversation:
> "Veo al menos dos lecturas posibles de esta pantalla: (1) [interpretación A], (2) [interpretación B]. ¿Cuál resuena más, o hay otra?"

**Component doesn't map cleanly:**
Never force a mapping. Say explicitly: "Este componente no tiene un equivalente directo en tu caso de uso." Then offer: remove it, replace it with something new, or keep it as pure decoration.

**Language:**
Responde en el idioma en que el usuario escribe. Si el usuario escribe en español, responde en español. Si escribe en inglés, responde en inglés.

## Output Templates

When the mock is approved, write these two files to the **active project directory** (wherever Claude Code is running):

### File 1: `docs/rock-this-shut/rock-this-shut-spec.md`

    # rock-this-shut Spec
    **Date:** [YYYY-MM-DD]
    **Reference:** [filename or description of input image/GIF]

    ## Origin Analysis
    ### Components
    [Full component list from DECODE with user edits applied]

    ### Holistic Interpretation
    [Synthesis paragraph from the DECODE conversation]

    ## Semantic Transposition
    ### New Use Case
    [User's stated new use]

    ### Component Mapping
    | Component | Original Role | New Role | Design Decision |
    |-----------|--------------|----------|-----------------|
    | A | [original] | [new] | [decision] |
    | B | [original] | [new] | [decision] |

    ## Technical Spec
    ### Colors
    [Hex values with semantic roles]

    ### Typography
    [Font, sizes, weights per component]

    ### Animations
    [Timing, easing, keyframe descriptions per animated component]

    ### Layout
    [Container, positioning, dimensions]

    ## Technical Decisions
    [Libs, tools, stack with justifications]

### File 2: `docs/rock-this-shut/rock-this-shut-plan.md`

    # rock-this-shut Development Plan
    **Date:** [YYYY-MM-DD]

    ## Resources to Acquire
    - [ ] [Library or asset name] — [why needed] — [source/install command]

    ## Tasks
    ### Task 1: [Component name]
    - [ ] [Step]
    - [ ] [Step]
    **Done when:** [criteria]

    ### Task 2: [Component name]
    - [ ] [Step]
    - [ ] [Step]
    **Done when:** [criteria]

    ## Integration Notes
    [How to plug this component into the target project]
