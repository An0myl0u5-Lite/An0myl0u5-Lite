# An0Matr1X

An0Matr1X is a proposed README-safe SVG motion engine for profile banners and repository headers.

It starts from the idea of `readme-typing-svg`, but the goal is to move far beyond typing and into lane-based, deterministic, multi-line motion scenes that still work inside GitHub READMEs.

---

## Core idea

Instead of "typing text", An0Matr1X builds text by dropping characters from vertical lanes into a landing zone.

Each character:
1. descends from a chosen lane,
2. optionally flickers through fake Matrix-style glyphs,
3. lands in the center column,
4. shifts older characters left by one cell,
5. continues until a line is complete.

This creates a hybrid of:
- Matrix glyph rain,
- terminal typography,
- old digital watch / Casio / G-Shock rhythm,
- grid-based "Tetris-like" sequencing.

---

## Why not just use readme-typing-svg?

Because the current engine is a typing illusion, not a lane compositor.

The stock parameters are useful for simple text banners, but they do not support:
- line fill order,
- lane selection,
- center-land-then-shift behavior,
- deterministic glyph rain,
- preset visual scenes,
- future sinkhole / collapse / overflow scenes.

So the plan is:
- preserve compatibility at the query-string edge,
- replace the animation engine underneath,
- keep old "typing" as a legacy mode,
- add new effect modules.

---

## ELI5 model

Think of it like this:

- lanes = drop tubes
- center = landing pad
- cell width = character grid size
- line fill = which row gets filled next
- lane mode = which drop tube the next character uses
- preset = one pre-made visual style and timing pack

A line is not typed. It is assembled.

---

## V1 feature set

### Fill modes
- `top-down`
- `bottom-up`
- `middle-out`
- `custom`

### Lane modes
- `random`
- `roundrobin`
- `manual`

### Motion rules
- multi-line text
- center landing
- shove-left by one grid cell on each new landing
- optional line-complete pause
- deterministic loop

### Visual rules
- monospace only
- fake glyph flicker while falling
- dark backgrounds first
- clean GitHub-safe SVG output

---

## Example config model

```json
{
  "effect": "matrix-drop",
  "lines": ["AN0MYL0U5", "RESEARCH", "COLLECTIVE"],
  "lineFill": "middle-out",
  "laneMode": "roundrobin",
  "lanes": [3, 7, 11, 15, 19],
  "centerCol": 11,
  "cellWidth": 16,
  "dropMs": 140,
  "settleMs": 60,
  "shiftMs": 80,
  "charPauseMs": 20,
  "lineCompletePauseMs": 250,
  "loopPauseMs": 1000,
  "flickerFrames": 4,
  "glyphSet": "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&*+-=<>/",
  "theme": "research-purple"
}
```

---

## Public query parameters

Proposed query parameters:

- `effect=typing|matrix-drop`
- `lines=` text separated by `;`
- `lineFill=top-down|bottom-up|middle-out|custom`
- `lineOrder=` e.g. `2,1,0`
- `laneMode=random|roundrobin|manual`
- `lanes=` e.g. `3,7,11,15,19`
- `laneMap=` e.g. `0,1,2,3|3,2,1,0`
- `centerCol=` integer
- `cellWidth=` integer
- `dropMs=` integer
- `settleMs=` integer
- `shiftMs=` integer
- `charPauseMs=` integer
- `lineCompletePauseMs=` integer
- `loopPauseMs=` integer
- `flickerFrames=` integer
- `glyphSet=` string
- `preset=` preset name

The preset layer should override timings, colors, lane defaults, and glyph style while still allowing manual overrides.

---

## Preset catalog

### 1. `gshock-retro`
Old digital-watch rhythm.

- palette: dark charcoal, muted green, amber accent
- movement: crisp, grid-snapped
- flicker: minimal
- vibe: vintage Casio / G-Shock LCD energy

### 2. `matrix-green`
Classic terminal rain feel.

- palette: black + phosphor green
- movement: slightly smoother descent
- flicker: moderate
- vibe: dense code-rain terminal

### 3. `research-purple`
Fit for the current An0myl0u5 / ARC profile identity.

- palette: black, violet, electric blue, soft green highlights
- movement: clean and deliberate
- flicker: moderate
- vibe: technical, modern, non-generic

### 4. `night-ops`
Security / command-line aesthetic.

- palette: black, slate, dim cyan, pale green
- movement: disciplined and sharp
- flicker: light
- vibe: threat intel / ops dashboard

### 5. `ember-alert`
High contrast warning / event mode.

- palette: black, deep orange, red, gold
- movement: aggressive
- flicker: high
- vibe: signal spike / live system alert

---

## Suggested default for the profile

For the main profile README:

- preset: `research-purple`
- effect: `matrix-drop`
- lineFill: `middle-out`
- laneMode: `roundrobin`
- lines:
  - `AN0MYL0U5`
  - `RESEARCH`
  - `COLLECTIVE`

This keeps the profile technical and distinctive without collapsing into noisy gimmick territory.

---

## Architecture

### 1. Compatibility layer
Accept existing `readme-typing-svg` style query parameters where possible.

### 2. Config normalizer
Parse user query strings into a single internal scene config.

### 3. Layout engine
Compute:
- line order,
- lane positions,
- center landing column,
- character grid positions,
- final settled positions.

### 4. Timeline compiler
Compute:
- drop start time,
- landing time,
- left-shift timing,
- line pause timing,
- loop reset timing.

### 5. SVG renderer
Emit declarative SVG only.

No JavaScript should be required for the main README-safe mode.

### 6. Effect modules
Separate the effects cleanly:
- `typing`
- `matrix-drop`
- later: `stack-fill`
- later: `sinkhole-collapse`

---

## GitHub profile integration plan

The main profile README currently uses two standard typing banners. Those can later be replaced with:

1. one primary An0Matr1X hero banner,
2. optionally one secondary stateless banner below it,
3. shared palette with the rest of the profile badges and project sections.

That gives a stronger identity and avoids the generic profile-banner look.

---

## Roadmap

### V1
- matrix-drop effect
- fill modes
- lane modes
- center landing
- shove-left motion
- preset system
- deterministic glyph flicker

### V1.1
- line completion pauses
- per-line timing offsets
- better preset browser/demo page
- manual lane maps by line

### V2
- text box fill / stack behavior
- more scene types
- overflow logic
- simple ASCII-inspired arrangements

### V3
- sinkhole collapse
- black-box fallthrough
- bottom impact sequence
- mushroom-cloud / pulse / reset scene

That V3 concept is intentionally deferred because it is a scene system of its own and should not contaminate the V1 renderer.

---

## Immediate next steps

1. Fork upstream repository outside this profile repo.
2. Brand the deployed engine as `An0Matr1X`.
3. Preserve legacy typing mode for compatibility.
4. Build `matrix-drop` first.
5. Create a preset browser/demo page so users can discover parameters visually.
6. Replace the current typing banners in the profile README once the endpoint is ready.

---

## Notes

GitHub profile READMEs require the username-matching public repository to display a top-of-profile README, and GitHub supports SVG rendering in repositories and README contexts. Dynamic SVG endpoints are therefore still the correct delivery model for this project.
