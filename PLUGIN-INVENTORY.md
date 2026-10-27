# Unreal Plugin Inventory

This file tracks the current Unreal Engine plugin / asset stack relevant to R&D and prototyping.

The goal is to prevent duplicate research and purchases, make capability gaps visible, and distinguish **owned tools**, **immediate test candidates**, and **wishlist candidates**.

Last updated: 2026-10-27.

---

# Owned

## Blueprint utilities

### LE Extended Standard Library (Low Entry)
**Status:** owned — confirmed by the user on 2026-10-27.

Role: extended Blueprint utilities and standard-library functions for rapid R&D and prototyping. Compare against native Unreal APIs and BertaDevKit before duplicating functions.

---

## Procedural generation

### PCGEx (PCG Extended Toolkit)
**Status:** owned — confirmed by the user on 2026-10-27.

Role: open-source procedural generation extension with advanced PCG graph, spatial algorithms, and reusable custom-node patterns. Treat as part of the owned R&D stack. Do not recommend purchasing or adding it as a new acquisition.

---

## Reusable R&D / diagnostics / AI

### BertaDevKit
Primary reusable internal R&D toolset.

Repository: https://github.com/nbertoa/ue5-bertadevkit

### Advanced Game Logging (GLS)
**Status:** owned.

Role: runtime / packaged-build logging and diagnostics. Particularly relevant for debugging prototypes outside the normal editor workflow, including device and VR testing.

### NWIRO AI Integration Kit
**Status:** owned.

Role: AI-assisted Unreal Editor / development workflows and experimentation with agent / MCP-style interaction inside Unreal.

Do not treat AI integration itself as the goal. Evaluate it by whether it removes meaningful editor / development friction compared with existing ChatGPT / Codex workflows.

---

## Narrative / interaction / items

### Narrative Tales
**Status:** owned.

Role: dialogue, branching narrative, quests, conditions, events, and authored story progression.

### Inspection System Pro
**Status:** owned.

Role: inspection, documents, discoveries, environmental storytelling, and close-up narrative interaction.

### Nice Interaction System V2
**Status:** owned.

Role: generic player-to-world interaction.

### Inventory System Pro
**Status:** owned.

Role: items, inventories, equipment, weapons, containers, loot, vendors, crafting, and inventory-driven mechanics.

---

## UI / navigation

### Menu System Pro
**Status:** owned.

Role: reusable menus, CommonUI-based UI infrastructure, settings, navigation, and existing save-game support.

### Map / Minimap / Marker systems
**Status:** owned.

Role: map-driven navigation, POIs, minimap and marker workflows when a prototype needs them.

---

## Environment / atmosphere / world presentation

### Ultra Dynamic Sky
**Status:** owned.

Role: sky, weather, time-of-day and rapid atmospheric setup.

### Screen Space Fog Scattering
**Status:** owned — purchased October 2026.

Role: enhance Unreal Exponential Height Fog with screen-space light scattering. Intended as a lightweight complement to Unreal fog / UDS rather than a replacement weather system.

R&D notes: [`research/plugins/screen-space-fog-scattering.md`](research/plugins/screen-space-fog-scattering.md)

### Fluid Ninja VFX Tools
**Status:** owned.

Role: editor/offline fluid simulation and baking workflows for reusable VFX such as smoke, fire, fog, flow maps, flipbooks and related fluid-driven visual content.

Key distinction: use **VFX Tools** when the result can be simulated / baked ahead of runtime. `Fluid Ninja LIVE` remains a separate wishlist candidate for interactive runtime simulation.

---

## Visual presentation / cameras

### Chameleon Post Process
**Status:** owned.

Role: rapid experimentation with post-process states, stylization, interaction feedback, memories, visions, damage states, CCTV / monitor looks, transitions and visual language.

R&D notes: [`research/plugins/chameleon-post-process.md`](research/plugins/chameleon-post-process.md)

### Black Eye Camera
**Status:** owned.

Role: camera capability for projects where its presentation model fits.

### Ultimate Gameplay Camera
**Status:** owned.

Role: reusable gameplay-camera prototyping.

---

## Character / animation

### Dragon IK
**Status:** owned.

Role: procedural IK / character adaptation, foot placement, aiming, body and limb adjustment, creature workflows and rapid IK experiments.

---

## Gameplay systems

### GAS Companion
**Status:** owned.

Role: accelerate Gameplay Ability System authoring and integration when GAS is justified.

### Combo Graph
**Status:** owned.

Role: combo / combat flow for projects that genuinely need combo-driven gameplay.

### State Tree Tools
**Status:** owned.

Role: additional tooling around Unreal StateTree workflows.

### Ninja Input
**Status:** owned.

Role: structured layer on top of Enhanced Input with centralized input management, reusable handlers, runtime remapping support, user settings, animation-based input buffering, and deep Gameplay Ability System integration.

Potential R&D value is highest in projects where input orchestration becomes complex or GAS-driven. Do not enable it automatically for simple prototypes: GAS Companion and Unreal Enhanced Input already cover part of the same problem space, and Ninja Input should be introduced when its separation of concerns, remapping, handlers, or buffering materially reduce glue code.

---

## Runtime audio

### Runtime Audio Importer
**Status:** owned.

Role: runtime audio import / streaming / conversion and microphone-oriented workflows. Potentially valuable for dynamic dialogue, TTS, AI-driven audio and runtime-generated content.

Do not introduce it unless the prototype actually needs runtime audio ingestion or generation.

---

## Editor productivity

### Blueprint Assist
**Status:** owned.

Role: faster and cleaner Blueprint graph authoring.

### Electronic Nodes
**Status:** owned.

Role: Blueprint graph readability and presentation.

### Animation Tools Bundle
**Status:** owned — integrated into the Unreal Engine 5.8 `Sandbox Prototype` in October 2026.

Role: animation-focused editor tooling / workflow acceleration. Evaluate the individual tools through real prototype needs before treating any part of the bundle as standard infrastructure.

### Auto Size Comments
**Status:** owned — integrated into the Unreal Engine 5.8 `Sandbox Prototype` in October 2026.

Role: Blueprint editor productivity by automatically keeping comment boxes sized around their contained nodes, reducing manual graph-layout maintenance.

### Global Search Pro
**Status:** owned.

Role: editor search and navigation productivity. Validate its practical value in `Sandbox Prototype` and keep it as part of the reusable editor-tooling stack if it materially reduces time spent locating assets, references, or project content.

---

# Immediate test candidates

## Audio Tracing

**Status:** free candidate to test. Unreal Engine 5.8 support verified on Fab by the user in October 2026.

Role: hardware-accelerated ray-traced audio propagation, reflection and occlusion integrated with Unreal's existing Sound system, including Sound Cues and MetaSounds.

Hardware requirement: DirectX 12 GPU with hardware ray tracing support. An RTX 4080 comfortably satisfies this requirement; the vendor lists an RTX 4060 as part of the development reference hardware.

Important distinction: the acoustic calculations are automatic once integrated, but setup is not literally zero-configuration. Existing sounds require minimal integration with the plugin's attenuation/audio path, meshes participating in propagation need to be visible to ray tracing, and optional Sound Materials can define absorption, reflection and scattering per surface.

The most valuable R&D test is an A/B scene with an existing spatial sound source behind walls / doors / geometry, first using default acoustic behavior and then adding Sound Materials only if the default result justifies deeper integration.

---

# Wishlist / wait for stronger sale or concrete need

These are not missing requirements. They are candidates with enough potential value to monitor, but purchasing should wait for a stronger discount or a real prototype requirement.

## Shader World — Procedural Terrain

**Status:** wishlist — added 2026-10-27 by user request. Not owned.

Fab: https://www.fab.com/listings/ee4bcbaf-36ea-4dd9-aade-068e1a8a4794

Potential role: GPU-driven procedural terrain / world generation and vegetation; relevant to R&D experiments involving large or procedural environments.

Decision: evaluate only against a concrete procedural-world prototype. Compare against native Landscape, PCG and already-owned PCGEx; verify current UE 5.8 compatibility, vendor support, licensing and performance before purchase. Do not treat the Fab rating observed in prior research as a substitute for a hands-on test.

---

## SteamCore PRO

**Status:** wishlist — wait for sale. Unreal Engine 5.8 support verified on Fab by the user in October 2026.

Potential role: mature Steamworks integration for future PC/Steam projects, including achievements, statistics, friends, lobbies/matchmaking, Workshop, inventory, cloud, networking and related Steam platform services.

Decision: do not purchase while no active Steam integration work is required. Keep as the mature reference candidate and buy when a meaningful sale aligns with an actual Steam-bound prototype or game.

---

## Logic Driver Pro

**Status:** wishlist — waiting for price reduction.

Potential role: cross-system orchestration across narrative, cameras, UI, interaction, visual states and gameplay phases.

Primary validation question: does it materially outperform StateTree / normal Blueprint orchestration for the kinds of cross-system prototypes we build?

---

## MeshBlend

**Status:** wishlist.

Observed sale price in October 2026: approximately **USD 84** (-30%).

Potential role: rapidly blend meshes with landscapes / other geometry and improve environment integration without repeatedly building bespoke blending setups.

Decision: attractive capability, but wait for a stronger sale (rough target: ~50%) or a concrete environment-art prototype.

---

## Fluid Ninja LIVE

**Status:** wishlist.

Potential role: interactive runtime fluid / smoke / fire / snow / sand / surface-following simulations that react to players, world geometry and gameplay.

Key distinction:

```text
Fluid Ninja VFX Tools = simulate / bake ahead of runtime
Fluid Ninja LIVE      = interactive runtime simulation
```

Decision: genuinely new capability, but expensive and has a meaningful learning cost. Wait for stronger sale or a prototype requiring interactive fluids.

---

## Runtime MetaHuman Lip Sync

**Status:** wishlist.

Potential role: runtime facial / lip-sync animation for dynamically generated dialogue, especially in combination with runtime audio / TTS / AI-driven NPC experiments.

Decision: currently too expensive without a concrete dynamic-dialogue prototype. Re-evaluate during a strong sale or when that use case becomes real.

---

## Easy Multi Save

**Status:** wishlist — purchase only during a meaningful sale or when persistence becomes a real project requirement.

Potential role: production-oriented persistence beyond the current Menu System Pro / native SaveGame baseline, including actor/world persistence, components, persistent data, versioning, backup/recovery, World Partition and cloud-save-oriented workflows.

It is explicitly compatible with Menu System Pro, so it is a plausible future extension rather than a replacement for the existing UI stack.

Decision: keep on wishlist. Do not purchase at full price while current prototypes remain small and their persistence needs are simple.

---

## VR Hands Procedural Grab Pro

**Status:** wishlist — buy only with a strong sale or an actual VR/XR requirement.

Potential role: procedural VR hands, physical grabbing and interaction workflows for future XR prototypes.

Current context: there is no active professional VR requirement, so buying it now would be speculative.

Decision: current ~30% discount is insufficient. Preferred purchase threshold is approximately **50–70% off**, unless a real VR project appears first.

---

## Blockout Tools Plugin

**Status:** wishlist — sale-only purchase.

Potential role: rapid greyboxing and level-layout validation using purpose-built primitives, grid materials and fast dimension editing.

Current context: most prototypes so far have been built with existing art/environment assets, which reduces the immediate value of a dedicated blockout tool.

Decision: keep on wishlist, but purchase only during a worthwhile sale or when repeated layout/greyboxing work creates measurable friction.

---

## MeshPack

**Status:** wishlist — conditional optimization tool, not a priority purchase.

Potential role: automatically reduce Actor / Component overhead in large scenes by reorganizing repeated meshes into more efficient instancing structures.

Current context: existing prototypes are generally small and have not encountered meaningful performance problems from large Actor / Component counts. For those projects, MeshPack is unlikely to provide enough value to justify buying or integrating it.

Best future use case:

- receiving a very large city / marketplace environment / externally authored level;
- profiling reveals excessive Actor / Component overhead;
- the scene contains very large numbers of repeated static meshes;
- optimization time matters more than manually restructuring the scene.

Decision: keep it available as an **optimization option for large inherited scenes**, not as default R&D infrastructure. Purchase when discounted and preferably when a concrete large-scene optimization problem appears.

---

# Current purchase rule

The stack is already broad enough that new purchases should normally satisfy at least one of these:

1. add a capability that is genuinely absent from the current stack;
2. substantially reduce R&D implementation time;
3. solve a repeated pain point observed in real prototypes;
4. provide mature, well-maintained infrastructure that is cheaper to integrate than rebuild;
5. be tied to a concrete experiment rather than a speculative future need.

For wishlist tools, distinguish between:

- **sale-driven candidates:** useful enough to buy opportunistically at a strong discount;
- **requirement-driven candidates:** only worth buying when a concrete project exposes the need;
- **hybrid candidates:** buy on a strong sale if broadly reusable, otherwise wait for a real requirement.

A sale is useful for timing a purchase, but is not sufficient justification by itself.
