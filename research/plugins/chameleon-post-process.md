# Chameleon Post Process — R&D Notes

## Purpose

This document captures the R&D conclusions that matter for using **Chameleon Post Process** in Unreal Engine prototypes.

It is intentionally **not a replacement for the official documentation**. Exact parameters, material behavior, compatibility details, and version-specific implementation should be checked against the current SUMFX documentation when needed.

The repository should preserve our conclusions, integration rules, and useful prototype patterns rather than duplicate vendor documentation.

---

## R&D role

**Primary role:** rapid experimentation with perception, mood, visual states, transitions, interaction feedback, and stylized presentation.

Chameleon is most valuable when a prototype needs to answer questions such as:

- How should a flashback or memory feel visually?
- How can a supernatural perception mode be communicated quickly?
- How can an interactable object be highlighted without building custom shaders first?
- How should damage, disorientation, CCTV, dreams, visions, or cinematic states look?
- Can a visual language be validated before investing in custom materials or rendering code?

Treat it as a **visual prototyping accelerator**, not foundational gameplay infrastructure.

---

## Architecture

### Chameleon Actor

The primary workflow is a `Chameleon` Blueprint Actor placed in the level.

It acts as a controller for a stack of post-process effects. Effects can be enabled, combined, reordered, and configured from the actor rather than requiring a separate Post Process Volume and material setup for each experiment.

Conceptually:

```text
Chameleon Actor
    │
    ├── Effect A
    ├── Effect B
    ├── Effect C
    │
    ↓
Post-process material stack
    ↓
Unreal rendering pipeline
```

This is the preferred workflow for rapid prototyping.

### Standalone effects

Modern Chameleon versions also allow effects to be used as standalone Unreal post-process materials.

Use this route when:

- integration with an existing Post Process Volume is cleaner;
- only one isolated material is needed;
- another system needs direct control of the post-process material;
- the Chameleon Actor would add unnecessary indirection.

Do not default to standalone materials merely because they are lower-level. The Chameleon Actor is usually faster for R&D.

### Recipes

Recipes are the preferred way to preserve reusable visual states.

Rather than modifying vendor assets, create project-owned presets / child configurations representing an intent:

```text
CR_Interaction
CR_Cinematic
CR_Vision
CR_Memory
CR_Damage
```

A Recipe should describe a **visual state or use case**, not simply repeat the name of an individual Chameleon effect.

### Chameleon Layers

Chameleon 16 introduces a layer-oriented editor workflow for building and ordering effect stacks. At the time of this research it is presented as a newer/beta workflow.

Useful for experimentation, but Recipes remain the more important architectural concept for reusable project-owned configurations.

### Runtime control

Chameleon is not editor-only. Effects and exposed Blueprint variables can be driven at runtime.

Typical pattern:

```text
Gameplay / narrative event
        ↓
Reference Chameleon
        ↓
Enable or modify effect
        ↓
Timeline / interpolation
        ↓
Return to normal state
```

This makes it suitable for transitions between narrative or gameplay states rather than only static art direction.

---

## Important shared capabilities

Many effects become substantially more useful because Chameleon provides reusable masking and pipeline controls around them.

### Blend masks

Use grayscale masks to restrict an effect to portions of the screen.

Useful for:

- partial overlays;
- damaged / obscured regions;
- stylized transitions;
- localized perception effects.

### Distance masking

Effects can be constrained or blended based on scene distance/depth.

Useful for:

- supernatural perception;
- fog-like stylization;
- separating foreground/background visual states;
- revealing distant or nearby information.

### Custom Depth

Custom Depth support is especially valuable for interaction and inspection prototypes because effects can target selected actors rather than the entire image.

This is one of the strongest integration points with:

- Nice Interaction System V2;
- Inspection System Pro;
- objective / clue highlighting;
- perception modes.

### Effect order and blendable location

Effect order matters: `A → B` does not necessarily produce the same result as `B → A`.

Recent Chameleon versions also expose blendable-location control within Unreal's post-process pipeline.

When a Recipe combines multiple effects, ordering is therefore part of the visual design and should be validated rather than treated as incidental.

---

# Effect priorities for our R&D work

Priority meanings:

- **S** — learn and prototype with it early; likely reusable.
- **A** — highly useful when the relevant problem appears.
- **B** — situational.
- **C** — mostly stylistic / niche; discover when needed.

Do **not** attempt to learn every Chameleon effect in advance. The value of the plugin is having a large toolbox available when a prototype creates a concrete need.

---

## S — Core effects worth knowing

### LOOK! Color Grading

**Use:** mood, scene identity, memories, dreams, time/state differentiation.

Probably the highest-value general-purpose effect because subtle color changes can communicate state without making the effect itself visible to the player.

### Custom Depth Highlighter

**Use:** interactables, clues, inspected objects, objectives, perception modes.

Strong candidate for integration experiments with Nice Interaction System and Inspection System Pro.

### Custom Depth Highlighter (Clip)

**Use:** selective highlighting where clipping behavior is useful.

Treat as a sibling capability of the standard highlighter rather than a separate visual language.

### Actor Featurette

**Use:** draw attention to a specific actor or temporarily separate it visually from the surrounding scene.

Potentially useful for inspection, tutorials, clues, narrative reveals, or interactable focus.

### Selective Color

**Use:** symbolic emphasis, clue highlighting, memories, perception states.

Particularly useful because it can be visually expressive without requiring a strongly synthetic-looking post effect.

### Blur

**Use:** transitions, focus, UI background treatment, dream/disorientation states.

Simple but highly reusable.

### Radial Blur

**Use:** revelation, focus, shock, supernatural perception, transitions.

Use subtly by default; exaggerated values quickly become visually intrusive.

### Letterboxing

**Use:** cinematic state communication and authored presentation.

Useful for rapid validation of whether entering a clearly defined cinematic mode helps an experience.

### Magic Transitions

**Use:** memory, dream, scene, state, inspection, or cinematic transitions.

High prototype value because transitions otherwise tend to require custom material work.

### Screen Decals

**Use:** screen-space graphics, overlays, contextual visual feedback.

Useful for rapidly testing HUD-adjacent ideas without first building a dedicated rendering solution.

### Screen Drops

**Use:** rain/water on lens, environmental immersion, transitions.

Useful as a quick atmospheric prototype before deciding whether the final experience needs a custom lens/environment implementation.

### Glitch

**Use:** digital failure, corrupted memory, monitor/CCTV language, transitions.

Common visual vocabulary; useful when justified by the fiction.

### TV Noise / Monitor Effects

**Use:** cameras, recorded footage, screens, surveillance, found media.

Especially useful for narrative storytelling because it immediately distinguishes mediated footage from normal gameplay.

### Hazy Lights

**Use:** dreams, visions, revelation, soft supernatural states, stylized atmosphere.

A strong candidate for subtle combinations rather than use as an isolated effect.

### Distance Fog / Gradient Fog

**Use:** visual separation, dream spaces, mood, rapid environment treatment.

Can complement Ultra Dynamic Sky: UDS changes the environment; Chameleon changes its visual perception/presentation.

### Lens Distortion

**Use:** camera identity, monitor/CCTV footage, subtle cinematic stylization, altered perception.

Prefer subtle values unless distortion itself is narratively meaningful.

---

## A — High-value situational effects

### Colorize

Useful for strong palette changes, visions, memories, warning states, or symbolic color treatment.

### Bleach Bypass

Useful for harsher cinematic / memory / footage looks.

### Monochrome

Useful for memories, CCTV, old footage, alternate perception states.

### Scratches

Useful for degraded film / recording / memory language.

### World Glitch

Useful when glitch should feel spatial rather than merely screen-space.

### Edge Detect

Useful for perception, scan, highlighting, stylized rendering experiments.

### Pulse

Useful as short-lived feedback or transition rather than a permanent look.

### Sonar

Useful for scans, reveals, perception mechanics, search modes.

### Screen Damage

Useful for validating damage feedback before building a project-specific damage material stack.

### Camera Shake

Useful for physical impact and feedback, though Unreal-native camera-shake systems should still be considered when the requirement is camera motion rather than visual post processing.

### Screen Indicator

Potentially useful for objective/interaction/gameplay indicators integrated into the visual presentation.

### Iridescent

Useful for supernatural, dream, otherworldly, or materialized perception states.

### Screen Waves

Useful for heat, dreams, supernatural distortion, water-like perception, transitions.

### Distance Distortion

Useful for spatially dependent altered perception.

### Snow / Frost

Useful for rapid climate/lens prototyping.

### Toon Shading / Comic / Sketch / Kuwahara

Useful for validating stylized art directions quickly.

Do not build a project around these merely because they are available. Use them when the visual style is part of the actual prototype question.

### Pixel Dissolve / 2D Transform

Useful for transitions and graphic manipulation of the rendered image.

### Haunted

Despite the name, potentially useful beyond horror if custom texture/configuration is used. Treat as a configurable supernatural/distortion tool rather than a genre-specific requirement.

---

## B — Situational toolbox

Examples:

- Sharpen
- Anamorphic Lens Flares
- Colored AO
- World Splash
- Screen Fire
- Actor Fire
- Digitize
- Cyber Scan
- Dither
- Mosaic
- Channel Swapper
- Channel Clamper
- Hue Panner
- Invert
- Mojo
- Zone
- Alcohol
- Drug
- Infected
- Melted World
- Alarm
- Neon
- Grid Cell

These can be useful, but there is little value in studying them before a prototype creates the requirement.

---

## C — Mostly niche / discover on demand

Examples:

- ASCII
- Kaleidoscope
- Emboss / Hard Emboss
- Circles
- Squares
- Tiles
- Pyramid
- Disco Ball
- Speed Lines
- Wired

This classification is not a judgment of implementation quality. It only reflects expected reuse in our current R&D direction.

---

# Recommended first Recipes

## `CR_Interaction`

Goal: make interaction focus readable without creating a custom interaction shader stack.

Candidate stack:

```text
Custom Depth Highlighter
+ subtle Selective Color
```

Experiment with Nice Interaction System V2 and Inspection System Pro.

Questions to validate:

- Is highlighting needed for every interactable or only focused/important ones?
- Does outline/fill compete with the intended art direction?
- Should inspection mode modify the world as well as the inspected object?

---

## `CR_Cinematic`

Goal: clearly enter an authored cinematic presentation state.

Candidate stack:

```text
LOOK! Color Grading
+ Letterboxing
+ subtle Lens Distortion
```

Keep the default implementation understated. The state change should serve the scene rather than advertise the post-process stack.

---

## `CR_Vision`

Goal: prototype visions, revelation, supernatural perception, or spiritually significant visual states.

Candidate stack:

```text
LOOK! Color Grading
+ Colorize
+ Hazy Lights
+ subtle Radial Blur
+ Pulse during transition
```

This is especially relevant to experimental audiovisual / interactive storytelling work.

Prefer a coherent, subtle language over stacking obvious psychedelic effects.

---

## `CR_Memory`

Goal: differentiate memory/flashback material from present-time gameplay.

Candidate stack:

```text
Monochrome or Bleach Bypass
+ Scratches
+ subtle Blur
+ optional TV Noise / Monitor Effects
```

The exact stack should depend on whether the memory is meant to feel like lived recollection, recorded footage, damaged memory, or stylized narration.

---

## `CR_Damage`

Goal: validate readable damage / critical-state feedback.

Candidate stack:

```text
Screen Damage
+ subtle Blur / Radial Blur
+ short Pulse
```

Avoid making post-process damage effects persistent enough to harm readability or comfort.

---

# Integration decisions

## Keep vendor assets untouched

Do not modify Chameleon vendor assets directly.

Project-specific work should live in project-owned Recipes, child assets, wrappers, or configuration.

Reasons:

- vendor updates remain safer;
- experiments are clearly separated from third-party content;
- configurations can be migrated/reused deliberately;
- it becomes clear which parts of the result are ours.

## Do not move Chameleon into BertaDevKit yet

Chameleon itself remains a third-party dependency.

Only add something to BertaDevKit if repeated projects reveal a stable reusable abstraction, for example:

```text
PlayVisualState(Damage)
PlayVisualState(Vision)
BlendVisualState(Memory, Duration)
```

or a generic Gameplay Tag / event-driven adapter that is genuinely useful across projects.

Do not wrap the plugin speculatively.

## Prefer intent-level APIs if an adapter becomes justified

A future reusable integration should expose semantic states such as `Damage`, `Vision`, or `Memory`, not Chameleon-specific effect names.

This keeps gameplay/narrative code independent from the implementation of the visual treatment.

## Validate Forward Rendering / VR separately

Many post-process techniques depend on buffers/features associated with deferred rendering. Chameleon's usefulness must be evaluated separately for Forward Rendering and VR/XR prototypes.

Do not assume a desktop/deferred Recipe will transfer unchanged to VR.

---

# First integration experiment

Highest-value initial experiment:

```text
Nice Interaction System V2
        ↓
focused interactable
        ↓
Custom Depth
        ↓
Chameleon Custom Depth Highlighter
        ↓
optional Selective Color / Actor Featurette
```

Then test the same visual language when entering Inspection System Pro.

This will answer a concrete question: **can Chameleon provide a reusable visual interaction language across generic interaction and narrative inspection without custom shader development?**

---

# Documentation strategy

Use this file for:

- R&D conclusions;
- priority effects;
- integration decisions;
- prototype recipes;
- lessons learned from experiments.

Use official SUMFX documentation for:

- exact effect parameters;
- current effect count;
- compatibility;
- version-specific behavior;
- Blueprint/property names;
- rendering details;
- changelog information.

At the time of the October 2026 review, SUMFX marketing/documentation and the effect index/changelog were not perfectly synchronized about the exact number/list of effects. Do not encode the vendor effect count as a critical architectural assumption.

Official references:

- Product: https://sumfx.net/products/chameleon-post-process/
- Documentation: https://sumfx.net/docs/chameleon/
- General / Getting Started: https://sumfx.net/docs/chameleon/general-chameleon/getting-started/
- Recipes: https://sumfx.net/docs/chameleon/general-chameleon/recipes/
- Blueprint Interaction: https://sumfx.net/docs/chameleon/general-chameleon/blueprint-interaction/
- Changelog: https://sumfx.net/docs/chameleon/general-chameleon/changelog/

---

## Current conclusion

Chameleon is valuable not because every included effect is individually important, but because it provides a broad, configurable visual toolbox that drastically lowers the cost of testing a visual idea.

For our R&D work, the highest-value areas are:

1. interaction / inspection highlighting;
2. cinematic presentation;
3. memories / flashbacks / recorded media;
4. visions / altered perception / supernatural states;
5. transitions;
6. rapid mood and atmosphere iteration.

The correct workflow is therefore:

**prototype with Chameleon → identify a useful visual language → preserve it as a project-owned Recipe → only build custom rendering or BertaDevKit integration when repeated use justifies it.**
