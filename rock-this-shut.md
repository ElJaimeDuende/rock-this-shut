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

3. **Always injects the Annotation System** (see Annotation System Template section) before `</body>` in every generated mock. No exceptions.

4. After showing the mock, says:
   > "Abrí el mock en tu browser. Usá el botón **✏ ANNOTATE** para marcar ajustes directamente sobre el mock — hacé click en cualquier elemento, escribí el cambio, y cuando termines clickeá **Copy for Claude** y pegá las anotaciones aquí."

### Phase 4.2 — ITERATE

**Trigger:** User pastes annotations in `[n] comment` format (output of "Copy for Claude" button).

**Claude does:**

1. Parses each annotation. Maps each `[n]` to the visual element the user clicked (infer from comment context + position in the component list).

2. For each annotation, identifies:
   - Which component it affects (A, B, C... from DECODE)
   - What visual property to change (color, size, font, spacing, layout, text, etc.)
   - The specific new value or direction

3. Applies all changes to the mock HTML file.

4. Re-injects the Annotation System clean (no previous pins — user starts fresh).

5. Shows the new mock version and says:
   > "Versión [N+1] lista. Revisá con ANNOTATE o confirmá si está aprobado."

6. Repeats Phase 4.2 until the user approves.

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

## Annotation System Template

Always append this block before `</body>` in every generated mock. It provides an in-browser annotation UI for the user to mark changes and send them back to Claude.

```html
<!-- ANNOTATION SYSTEM — rock-this-shut v2 -->
<style>
  #ann-toggle {
    position: fixed; top: 16px; right: 16px;
    background: #00FF41; color: #080808;
    font-family: 'Orbitron', monospace; font-size: 11px; font-weight: 700;
    letter-spacing: 0.1em; padding: 8px 18px;
    cursor: pointer; z-index: 1000; user-select: none;
    border: 1px solid #00FF41;
  }
  body.ann-mode { cursor: crosshair; }
  body.ann-mode #ann-toggle { background: #080808; color: #00FF41; }
  #ann-panel {
    position: fixed; top: 56px; right: 16px; width: 260px;
    background: #0d0d0d; border: 1px solid #00FF41; color: #00FF41;
    font-family: monospace; font-size: 11px;
    z-index: 999; display: flex; flex-direction: column;
    max-height: calc(100vh - 80px);
  }
  #ann-panel-header {
    padding: 8px 12px; border-bottom: 1px solid #00CC33;
    font-size: 10px; letter-spacing: 0.12em; color: #00CC33; text-transform: uppercase;
  }
  #ann-list { flex: 1; overflow-y: auto; padding: 10px 12px; }
  .ann-empty { color: #00CC33; line-height: 1.8; }
  .ann-item { display: flex; gap: 8px; margin-bottom: 10px; align-items: flex-start; }
  .ann-num {
    background: #00FF41; color: #080808; font-size: 10px; font-weight: 700;
    min-width: 20px; height: 20px; display: flex; align-items: center;
    justify-content: center; flex-shrink: 0;
  }
  .ann-text { color: #00FF41; line-height: 1.5; }
  #ann-panel-actions { padding: 8px 12px; border-top: 1px solid #00CC33; display: flex; flex-direction: column; gap: 6px; }
  #ann-copy, #ann-clear {
    background: transparent; border: 1px solid #00FF41; color: #00FF41;
    font-family: monospace; font-size: 9px; letter-spacing: 0.1em;
    padding: 6px; cursor: pointer; width: 100%; text-transform: uppercase;
  }
  #ann-copy:hover { background: #00FF41; color: #080808; }
  #ann-clear:hover { border-color: #ff4141; color: #ff4141; }
  .ann-pin {
    position: fixed; width: 22px; height: 22px;
    background: #00FF41; color: #080808; font-size: 10px; font-weight: 700;
    display: flex; align-items: center; justify-content: center;
    z-index: 1001; transform: translate(-50%, -50%);
    pointer-events: none; box-shadow: 0 0 8px #00FF41;
  }
  .ann-input-box {
    position: fixed; width: 260px; background: #0d0d0d;
    border: 1px solid #00FF41; padding: 10px; z-index: 1002;
    box-shadow: 0 0 12px rgba(0,255,65,0.3);
  }
  .ann-input-box textarea {
    width: 100%; background: #080808; border: 1px solid #00CC33; color: #00FF41;
    font-family: monospace; font-size: 11px; padding: 6px; resize: none; outline: none; line-height: 1.5;
  }
  .ann-input-actions { display: flex; gap: 6px; margin-top: 6px; }
  .ann-save, .ann-cancel {
    flex: 1; background: transparent; border: 1px solid #00FF41; color: #00FF41;
    font-family: monospace; font-size: 9px; letter-spacing: 0.1em;
    padding: 5px; cursor: pointer; text-transform: uppercase;
  }
  .ann-save { background: #00FF41; color: #080808; }
  .ann-cancel:hover { border-color: #ff4141; color: #ff4141; }
</style>

<div id="ann-toggle" onclick="toggleAnnotation()">✏ ANNOTATE</div>
<div id="ann-panel">
  <div id="ann-panel-header">Annotations</div>
  <div id="ann-list"><p class="ann-empty">Click ANNOTATE, then<br>click any element to comment.</p></div>
  <div id="ann-panel-actions">
    <button id="ann-copy" onclick="copyAnnotations()">Copy for Claude</button>
    <button id="ann-clear" onclick="clearAnnotations()">Clear All</button>
  </div>
</div>

<script>
  let annotationMode = false, annotations = [], nextId = 1, activeInput = null;
  function toggleAnnotation() {
    annotationMode = !annotationMode;
    document.body.classList.toggle('ann-mode', annotationMode);
    document.getElementById('ann-toggle').textContent = annotationMode ? '✓ DONE' : '✏ ANNOTATE';
  }
  document.addEventListener('click', function(e) {
    if (!annotationMode) return;
    if (e.target.closest('#ann-toggle') || e.target.closest('#ann-panel') || e.target.closest('.ann-input-box')) return;
    if (activeInput) { activeInput.remove(); activeInput = null; }
    const id = nextId++, x = e.clientX, y = e.clientY;
    const pin = document.createElement('div');
    pin.className = 'ann-pin'; pin.textContent = id;
    pin.style.left = x + 'px'; pin.style.top = y + 'px';
    document.body.appendChild(pin);
    const box = document.createElement('div');
    box.className = 'ann-input-box';
    box.style.left = Math.min(x + 20, window.innerWidth - 280) + 'px';
    box.style.top = Math.min(y - 10, window.innerHeight - 130) + 'px';
    box.innerHTML = '<textarea rows="3" placeholder="Describe the change..."></textarea><div class="ann-input-actions"><button class="ann-save">Add</button><button class="ann-cancel">Cancel</button></div>';
    document.body.appendChild(box); activeInput = box;
    setTimeout(() => box.querySelector('textarea').focus(), 10);
    box.querySelector('.ann-save').addEventListener('click', function() {
      const text = box.querySelector('textarea').value.trim();
      if (text) { annotations.push({id, comment: text}); updatePanel(); } else { pin.remove(); nextId--; }
      box.remove(); activeInput = null;
    });
    box.querySelector('.ann-cancel').addEventListener('click', function() { pin.remove(); nextId--; box.remove(); activeInput = null; });
    box.querySelector('textarea').addEventListener('keydown', function(e) {
      if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); box.querySelector('.ann-save').click(); }
      if (e.key === 'Escape') { box.querySelector('.ann-cancel').click(); }
    });
  });
  function updatePanel() {
    const list = document.getElementById('ann-list');
    if (!annotations.length) { list.innerHTML = '<p class="ann-empty">No annotations yet.</p>'; return; }
    list.innerHTML = annotations.map(a => '<div class="ann-item"><span class="ann-num">' + a.id + '</span><span class="ann-text">' + a.comment + '</span></div>').join('');
  }
  function copyAnnotations() {
    if (!annotations.length) return;
    navigator.clipboard.writeText(annotations.map(a => '[' + a.id + '] ' + a.comment).join('\n')).then(() => {
      const btn = document.getElementById('ann-copy');
      btn.textContent = 'Copied!';
      setTimeout(() => btn.textContent = 'Copy for Claude', 2000);
    });
  }
  function clearAnnotations() {
    annotations = []; nextId = 1;
    document.querySelectorAll('.ann-pin').forEach(p => p.remove());
    updatePanel();
  }
</script>
<!-- END ANNOTATION SYSTEM -->
```

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
