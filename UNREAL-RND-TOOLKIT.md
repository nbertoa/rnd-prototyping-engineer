# Unreal R&D Toolkit

This document tracks the current reusable Unreal Engine toolkit available for professional prototyping and personal R&D experiments.

The purpose is not to collect plugins. The toolkit exists to reduce repeated implementation work, shorten the path from an ambiguous requirement to a testable prototype, and make it easier to focus engineering effort on the parts that actually require custom work.

The guiding rule remains:

**Unreal native → mature existing solution → integrate / extend → build custom only when justified.**

---

## 1. Reusable R&D Infrastructure

### BertaDevKit

**Role:** primary reusable R&D toolset.

Use it for generic tooling, diagnostics, integrations, runtime utilities, editor workflows, hardware/software interaction, and reusable capabilities discovered through real prototype work.

Repository: https://github.com/nbertoa/ue5-bertadevkit

BertaDevKit should not absorb third-party systems simply because they are useful. Prefer optional adapters or integrations when repeated use demonstrates real value.

---

## 2. Narrative / Storytelling

### Narrative Tales

**Role:** authored dialogue, branching narrative, quests, narrative events, conditions, and story progression.

Use it when the problem is fundamentally narrative rather than trying to recreate dialogue or quest infrastructure in project-specific Blueprints.

### Inspection System Pro

**Role:** inspectable objects, documents, discoveries, audio / environmental storytelling, and close-up interaction with narrative objects.

It complements generic interaction rather than replacing it.

### Logic Driver Pro — not owned yet

**Potential role:** high-level visual orchestration across otherwise independent systems.

Possible uses:

- sequence experience phases;
- coordinate narrative, cameras, lighting, UI, gameplay state, and interaction;
- author reproducible R&D / validation workflows;
- build designer-facing orchestration without giant manager Blueprints.

Logic Driver is still a candidate, not part of the current owned stack. Its value should be validated against StateTree and existing Unreal-native approaches before treating it as infrastructure.

---

## 3. Interaction / Items / Equipment

### Nice Interaction System V2

**Role:** generic player-to-world interaction.

Typical uses:

- doors and switches;
- contextual interaction;
- pickups;
- world objects;
- generic interactables.

### Inventory System Pro

**Role:** items, inventories, equipment, weapons, containers, loot, vendors, crafting, and other inventory-driven mechanics.

This is useful both for personal prototypes and professional briefs where inventory / equipment would otherwise require a substantial amount of setup before validating the actual gameplay idea.

### Relationship between the two

```text
Nice Interaction System
        ↓
Player interacts with the world
        ↓
Inventory System Pro
        ↓
Item ownership / equipment / item state
```

Inspection System remains separate because inspecting an object as part of storytelling is not the same problem as generic interaction or inventory ownership.

---

## 4. UI / Navigation / Save

### Menu System Pro

**Role:** reusable menu / UI infrastructure, settings, navigation, and its existing save-game management capabilities.

Do not add another save framework until a real prototype demonstrates that the existing solution or native `SaveGame` is insufficient.

### Map / Minimap / Marker systems

**Role:** spatial navigation, points of interest, markers, and map-driven gameplay when the project actually needs them.

Do not enable a map by default. Environmental navigation may be the better design for short or strongly authored experiences.

### Objective Path Guide — owned and integrated in an active project (startup issue pending)

**Role:** world-space visual route guidance for an identified destination, using NavMesh and Niagara-rendered spline / moving orb / ground effects. Ideal for an occasional, temporary hint that appears, guides and fades out. Distinct from the map/minimap UI.

**Documented:** `Guide Component` on player; `Start Guidance To`, `Start Guidance To Via`, `Stop Guidance`; `Orb Guide` / `Spline Guide` / `Both`; NavMesh-dependent paths, deviation-based updates, reveal/fade controls, optional arrival disappearance.

**Integration rule:** start with its built-in Niagara effects; add a project-controlled timer only if temporary hint behavior is required. Do not claim arbitrary custom Niagara replacement, UE 5.8 compatibility, packaged-build behavior or Map System Pro interoperability until independently tested. No need for an automatic BertaDevKit wrapper.

**Observed integration (user report 2026-10-28):** Guide Component added to the player, `Guiding Mode = Both`, target actor in the level, and automatic `Start Guidance To` from BeginPlay. Navigation works after a ~0.2 s delay, but `PathSpline` is initially `None` without it, and Niagara is briefly visible without a valid path. Treat this as integrated with a startup visual defect, not a finished or packaged-validated implementation. The user also observed that disabling Blueprint **Context Sensitive** revealed the native seconds-based `Delay` when LE Extended Standard Library affected node discovery; root cause unverified. Project identity and UE version still unconfirmed.

**Source and implementation guide:** [Objective Path Guide technical reference](research/plugins/objective-path-guide.md).

---

## 5. Environment / Atmosphere

### Ultra Dynamic Sky

**Role:** reusable sky, time-of-day, weather, environmental atmosphere, and rapid outdoor scene setup.

High-value R&D uses:

- quickly establish a convincing outdoor prototype;
- test day / night progression;
- prototype rain, snow, storms, fog, wind, and weather transitions;
- drive environmental changes from narrative or gameplay state;
- avoid repeatedly rebuilding the integration between Unreal's individual atmospheric systems.

This is considered a highly reusable environment capability, not just a visual asset.

---

## 6. Visual Presentation

### Chameleon Post Process

**Role:** rapid experimentation with perception, mood, visual states, and stylized post-processing.

Possible storytelling / gameplay uses:

- flashbacks;
- dreams;
- damage states;
- supernatural perception;
- CCTV / monitor effects;
- horror / distortion states;
- stylized rendering experiments.

It is best treated as a prototyping and presentation accelerator rather than foundational gameplay infrastructure.

Important constraint: many effects depend on deferred-rendering buffers, so its value may be lower in Forward Rendering / VR scenarios.

Detailed R&D notes, architecture, priority effects, Recipes, and integration decisions: [`research/plugins/chameleon-post-process.md`](research/plugins/chameleon-post-process.md).

### Black Eye Camera

**Role:** existing camera capability for projects where its approach fits the required presentation.

### Ultimate Gameplay Camera

**Role:** reusable gameplay-camera capability and rapid camera prototyping.

### Sequencer

Use Unreal-native Sequencer when the problem is authored cinematic timing rather than runtime gameplay-camera logic.

---

## 7. Character Animation / Procedural Adaptation

### Dragon IK

**Role:** procedural character adaptation and IK acceleration.

Useful areas include:

- foot placement on uneven terrain / stairs;
- aiming and body adaptation;
- arm / weapon IK;
- procedural adjustment without authoring an animation for every situation;
- creatures and non-standard characters;
- physical-animation experiments;
- VR full-body experiments.

Unreal already provides IK Rig, Control Rig, Full Body IK, Motion Warping, and related native tools. Dragon IK is valuable when it shortens implementation time or provides a solved workflow for a prototype; it should not automatically replace native systems.

---

## 8. Gameplay Systems

### GAS Companion

**Role:** accelerate Gameplay Ability System authoring / integration where GAS is justified.

### Combo Graph

**Role:** combo / combat flow where a project genuinely needs combo-driven gameplay.

Do not introduce GAS or Combo Graph into projects whose mechanics do not benefit from them.

---

## 9. State / Behavior Authoring

### StateTree + State Tree Tools

**Role:** Unreal-native state-driven behavior and gameplay logic, with additional tooling already available in the stack.

This remains the baseline to compare against before buying or adopting another state-machine / AI framework.

### Dedicated AI framework

No additional general-purpose AI framework is currently required.

Smart AI, AI Behavior Toolkit, Wise Feline, or similar products should only be reconsidered when a real prototype needs capabilities that would otherwise consume meaningful implementation time.

The absence of an AI plugin is currently intentional rather than a gap that must be filled immediately.

---

## 10. Editor / Authoring Productivity

### Blueprint Assist

**Role:** faster and cleaner Blueprint graph authoring.

### Electronic Nodes

**Role:** Blueprint graph readability / presentation.

These are authoring productivity tools rather than runtime dependencies.

---

# Capability Map

```text
                         R&D / PROTOTYPING
                                │
                ┌───────────────┴───────────────┐
                │                               │
          BertaDevKit                    Unreal native systems
                │
        reusable engineering
                │
 ┌──────────────┼──────────────────────────────────────────────┐
 │              │              │              │               │
Narrative   Interaction     Environment     Visual         Character
 │              │              │         Presentation      Animation
 │              │              │              │               │
Narrative    Nice          Ultra Dynamic   Chameleon       Dragon IK
Tales        Interaction   Sky             Cameras
 │              │
Inspection   Inventory
System       System Pro

UI / Navigation: Menu System Pro + Map / Minimap / Markers
Gameplay: GAS Companion + Combo Graph
State logic: StateTree + State Tree Tools
Future orchestration candidate: Logic Driver Pro
```

---

# Important Overlaps

## Inspection vs Nice Interaction

- **Nice Interaction:** generic interaction with the world.
- **Inspection:** examining / discovering narrative objects.

They complement each other.

## Nice Interaction vs Inventory

- **Interaction:** how the player acts on an object.
- **Inventory:** what happens once that object becomes an item owned / equipped / stored by the player.

They complement each other.

## UDS vs Chameleon

- **UDS:** changes the environment / physical atmosphere.
- **Chameleon:** changes how the player perceives / visually experiences it.

They operate at different layers and can be combined for narrative atmosphere.

## Dragon IK vs Unreal native IK

This is genuine overlap. Start from the cheapest / simplest native solution when appropriate and use Dragon IK when it measurably accelerates or improves the prototype.

## Logic Driver vs StateTree

This is the most important future comparison.

Do not buy or adopt Logic Driver merely because it is another visual state machine. Its strongest possible value is **cross-system orchestration**, especially when coordinating multiple plugins and R&D workflows.

---

# Current Gaps

These are not automatic purchase targets. They are areas to evaluate only when requirements make them relevant.

### AI / NPC simulation

No dedicated framework is currently justified. Start with Unreal-native AI, StateTree, Behavior Trees, EQS, Smart Objects, and existing tools. Re-evaluate only when a concrete brief exposes repeated friction.

### Advanced persistence

Menu System Pro and Unreal `SaveGame` should be tested first. A dedicated persistence framework is only justified by real world-state complexity, session requirements, versioning, or serialization needs.

### Traversal / locomotion framework

Do not buy a large movement framework speculatively. Evaluate it when a project requires climbing, parkour, grappling, swimming, or other traversal systems as a central mechanic.

### Logic Driver / orchestration

This is the clearest remaining candidate because it may connect multiple existing capabilities rather than duplicate one isolated feature. Wait for a favorable purchase opportunity or a real orchestration experiment before committing.

---

# Near-Term R&D Priority

The current bottleneck is no longer acquiring more plugins.

The next useful step is to **validate the stack through small integration experiments** and document what actually reduces prototype time.

Suggested experiments:

1. **Narrative interaction slice**
   - Nice Interaction → Inspection → Narrative Tales → Menu / save state.

2. **Atmospheric storytelling slice**
   - Narrative event → Ultra Dynamic Sky weather / time transition → Chameleon visual state → camera / Sequencer.

3. **Item / equipment slice**
   - World interaction → pickup → Inventory System Pro → equip / unequip → persistence.

4. **Character adaptation slice**
   - Standard UE character → uneven terrain / aiming / interaction → compare native IK against Dragon IK.

5. **Cross-system orchestration experiment**
   - Build once with StateTree / normal Blueprint orchestration.
   - Later compare the same problem with Logic Driver if purchased.

The purpose of these experiments is not to demonstrate every feature of every plugin. It is to learn where each tool saves time, where integrations become fragile, and which repeated glue should eventually become reusable BertaDevKit adapters or diagnostics.

---

# Rule for Future Purchases

A new plugin should normally satisfy at least one of these conditions:

1. It solves a recurring professional R&D problem substantially faster than Unreal-native tools.
2. It introduces a genuinely missing capability rather than duplicating the current stack.
3. It is mature, well maintained, documented, and sufficiently validated by real users.
4. It can be integrated without making the prototype architecture unnecessarily dependent on it.
5. There is a concrete experiment or likely brief in which it will be used.

A discount is not, by itself, a reason to buy a plugin.
