# R&D Lab Architecture

This document defines how professional client work, reusable tooling, and personal R&D experiments relate to each other.

The goal is to keep the profile coherent without mixing confidential client work, reusable public infrastructure, and domain-specific experiments.

## Three Connected Layers

### 1. Professional / Client R&D

Client work is where many real technical problems appear:

- ambiguous requirements;
- unfamiliar technologies;
- hardware and external integrations;
- gameplay and interaction problems;
- performance investigations;
- tooling needs;
- prototype validation;
- architecture and implementation tradeoffs.

Much of this work cannot be published directly because it may contain proprietary code, assets, requirements, internal decisions, or confidential information.

The value of this layer is therefore primarily professional experience and the discovery of reusable engineering patterns.

### 2. Reusable R&D Infrastructure — BertaDevKit

BertaDevKit is the reusable technical layer that can support both professional and personal R&D without carrying client-specific intellectual property.

It should contain or integrate capabilities that repeatedly reduce prototyping friction, improve diagnosis, make experiments more observable, or accelerate validation.

The direction is not to build every system internally. BertaDevKit should remain comfortable integrating mature third-party solutions when that is the better engineering decision.

Existing examples include optional extensions around GAS Companion, Combo Graph, Ultimate Gameplay Camera, and Black Eye Camera.

### 3. Personal R&D Laboratory

Personal projects provide a domain where complete experiments can be built, shown publicly, documented, and reused as evidence.

A major personal laboratory direction is **interactive / cinematic storytelling**, potentially including Christian or biblical experiences.

Storytelling is the application domain, not the professional identity. The R&D value comes from the technical questions investigated while building the experience.

Examples:

- how to prototype narrative experiences quickly;
- how to integrate commercial Unreal plugins without creating fragile dependencies;
- how to orchestrate dialogue, inspection, cameras, audio, cinematics, UI, gameplay state, weather, visual presentation, inventory, and interaction;
- how to build reusable authoring and diagnostic tools;
- how to decide what should stay project-specific and what deserves promotion into BertaDevKit;
- how AI can accelerate research, authoring, testing, and documentation.

## Information Flow Between Layers

The intended flow is:

```text
Client R&D problems
        ↓
Reusable lessons / generic capabilities
        ↓
BertaDevKit
        ↑
Personal R&D experiments
        ↓
Reusable tooling / integrations / evidence
```

Client-specific code or proprietary information must never move into the public reusable layer.

Personal experiments can safely act as proving grounds for reusable tooling before it is applied elsewhere.

---

# Logic Driver as a Potential Orchestration Layer

Logic Driver Pro is not currently owned and is not considered a replacement for StateTree, Narrative Tales, GAS, or any other domain-specific system.

Its potentially valuable role is different:

> **Logic Driver may provide a visual authoring and orchestration layer on top of BertaDevKit capabilities and optional integrations.**

The useful distinction is:

- **BertaDevKit knows how to perform operations.**
- **Logic Driver can describe when, in what order, under what conditions, and with what recovery behavior those operations should run.**

## Potential Architecture

```text
Third-party systems
    │
    ├── GAS Companion
    ├── Combo Graph
    ├── Black Eye Camera
    ├── Ultimate Gameplay Camera
    ├── Narrative Tales
    ├── Inspection System Pro
    ├── Inventory System Pro
    ├── Ultra Dynamic Sky
    ├── Chameleon
    └── Dragon IK
             │
             ▼
Optional BertaDevKit integration layers
             │
             ▼
     BertaLogicDriverExt (?)
             │
             ▼
      Logic Driver graphs
```

`BertaLogicDriverExt` is a **hypothesis to validate**, not a committed plugin.

It should only exist if real experiments demonstrate reusable value.

Logic Driver itself is commercial and must remain an optional local dependency. The public BertaDevKit base should not require it.

## What Should Remain in BertaDevKit / Integration Plugins

Keep low-level and atomic capabilities in normal C++ / Blueprint APIs:

- wrappers around third-party APIs;
- diagnostics;
- queries;
- tracing;
- validation primitives;
- lifecycle and ownership;
- hardware and process operations;
- reusable helpers;
- snapshots and state inspection;
- integration-specific adapters.

## What Logic Driver Could Orchestrate

Use visual graphs for higher-level behavior such as:

- multi-step workflows;
- experiment execution;
- branching and recovery;
- integration-test scenarios;
- diagnostic procedures;
- demo flows;
- experience-level orchestration;
- repeated validation sequences;
- designer-facing authoring workflows.

## Candidate R&D Tools Built with Logic Driver

### Berta Experiment Runner

Describe reproducible experiments visually:

```text
Setup
  ↓
Warmup
  ↓
Run
  ↓
Measure
  ↓
Validate
  ↓
Report
  ↓
Cleanup
```

Possible nodes:

- Start / stop trace
- Set CVar
- Spawn test actors
- Wait for stable FPS
- Record metric
- Capture screenshot
- Launch process
- Assert result
- Export report

### Hardware / Device Test Flow

Useful with BertaDualSense, BertaSerial, BertaProcessBridge, and BertaSystemInfo:

```text
Detect Device
  ↓
Connect
  ↓
Calibrate
  ↓
Execute Test
  ↓
Validate Response
  ↓
Pass / Recover / Fail
```

### Plugin Integration Test Harness

Create reproducible validation flows around commercial plugins.

Examples:

- verify an Inspection System discovery survives the intended lifecycle;
- start a Narrative Tales dialogue and validate resulting world state;
- grant a GAS ability, activate it, inspect tags/effects, and assert expected state;
- execute a camera transition and validate completion / configuration;
- change Ultra Dynamic Sky weather and validate narrative / environmental state;
- equip an Inventory System item and validate resulting gameplay state.

### Diagnostic Wizard

Represent diagnosis as a visible decision graph.

Example:

```text
ASC exists?
  ├── No → Report missing ASC
  └── Yes
        ↓
Ability granted?
        ↓
Tags valid?
        ↓
Cost / cooldown valid?
        ↓
Diagnosis
```

### Visual Validation Pipeline

Compose project checks, plugin checks, configuration validation, test execution, and reporting as a visible workflow instead of a collection of disconnected buttons.

### AI-Assisted Experiment Authoring

Logic Driver Graph Generation / Assist may provide a useful research surface for AI-assisted Unreal authoring.

Potential investigation:

> Can an AI agent generate or modify a reviewed experiment graph from a natural-language test specification while the engineer retains control over validation and execution?

This is experimental R&D, not infrastructure to depend on before it proves reliable.

---

# Logic Driver in the Storytelling Lab

Logic Driver should not replace domain-specific systems that already solve their problems well.

Preferred responsibilities:

- Narrative Tales → dialogue, branching narrative, quests;
- Inspection System Pro → inspectable objects, documents, discoveries, environmental storytelling;
- Nice Interaction System V2 → generic player-to-world interaction;
- Inventory System Pro → items, equipment, weapons, containers, loot, crafting, vendors;
- StateTree + State Tree Tools → NPC / AI behavior and state-driven gameplay where appropriate;
- GAS + GAS Companion → abilities, effects, attributes, tags;
- Combo Graph → combo / combat flow where required;
- Menu System Pro + CommonUI → UI and navigation;
- Map / Minimap / Marker systems → spatial navigation when required;
- Ultra Dynamic Sky → environment, weather, time-of-day, atmosphere;
- Chameleon → post-process language, perception states, visual mood;
- Dragon IK → procedural character adaptation / IK when it accelerates the prototype;
- Black Eye Camera / Ultimate Gameplay Camera / Sequencer → cameras and cinematic presentation;
- BertaDevKit → reusable R&D capabilities, diagnostics, integration, and tooling.

Logic Driver's strongest storytelling role would be **experience orchestration**.

Example:

```text
Intro
  ↓
Explore
  ↓
Wait for Discovery
  ↓
Start Dialogue
  ↓
Change Weather / Lighting
  ↓
Change Visual State
  ↓
Play Sequence
  ↓
New Exploration Phase
  ↓
Final Dialogue
  ↓
Ending
```

Each state delegates the domain work to the appropriate system instead of reimplementing it.

This avoids giant project-specific manager Blueprints and makes experimental flow easier to rearrange while requirements are changing.

## Potential Berta Logic Driver Nodes for Storytelling

Only create these when a real prototype needs them. Possible examples:

- Wait For Discovery
- Start / Wait For Dialogue
- Play / Wait For Sequence
- Set Camera Mode
- Fade Screen
- Wait For Gameplay Tag
- Play Voice Over
- Trigger Narrative Event
- Set Weather / Time Of Day
- Set Post Process State
- Wait For Item / Equipment State
- Validate Integration State

The purpose is not to create a giant node library speculatively. Nodes should emerge from repeated real needs.

---

# Current Storytelling / Prototyping R&D Stack

For the detailed capability map, overlaps, gaps, and suggested experiments, see [UNREAL-RND-TOOLKIT.md](UNREAL-RND-TOOLKIT.md).

## Already Available / Owned

### Core R&D / authoring

- BertaDevKit
- State Tree Tools
- Blueprint Assist
- Electronic Nodes

### Narrative / interaction / items

- Narrative Tales
- Inspection System Pro
- Nice Interaction System V2
- Inventory System Pro

### UI / navigation

- Menu System Pro
- Map / Minimap / Marker systems

### Gameplay

- GAS Companion
- Combo Graph

### Cameras / presentation

- Black Eye Camera
- Ultimate Gameplay Camera
- Ultra Dynamic Sky
- Chameleon Post Process

### Character animation / procedural adaptation

- Dragon IK

## High-Priority Candidate

### Logic Driver Pro

Logic Driver remains the clearest major candidate that could add a new layer rather than simply another isolated gameplay feature.

Its value is not primarily another gameplay state machine. The hypothesis is that it could become an extensible visual orchestration / authoring layer for BertaDevKit and for cross-plugin workflows.

It is not currently owned. Do not purchase or adopt it solely because it is powerful; validate the need against StateTree + State Tree Tools on a real orchestration problem, or acquire it when the combination of price and expected use makes the experiment reasonable.

## Requirement-Driven / Intentionally Deferred

### Dedicated AI framework

No dedicated general-purpose AI plugin is currently justified. Unreal-native AI, StateTree, Behavior Trees, EQS, Smart Objects, and existing tools should be the baseline.

Products such as Smart AI, AI Behavior Toolkit, or Wise Feline should only be reconsidered when a concrete prototype exposes a real need.

### Advanced Save System

Menu System Pro already provides save-game capabilities and Unreal provides native `SaveGame`. A dedicated persistence framework should only be added when actual world-state complexity, session requirements, versioning, cloud, or serialization needs justify it.

### Traversal / movement framework

Do not buy a large movement framework speculatively. Evaluate one when traversal itself becomes a central requirement.

---

# Current Integration Priority

The stack is now broad enough that the main R&D value should come from **using and integrating it**, not continuing to accumulate plugins.

Near-term experiments should answer practical questions:

1. How quickly can a narrative interaction move from Nice Interaction → Inspection → Narrative Tales?
2. How cleanly can Inventory System Pro connect world interaction, equipment, and persistence?
3. Can Ultra Dynamic Sky + Chameleon + camera systems create rapid atmospheric narrative transitions without fragile glue?
4. Where does Dragon IK materially outperform or accelerate Unreal-native IK workflows?
5. Which repeated integration friction deserves a BertaDevKit adapter, diagnostic, or reusable helper?
6. Does cross-system orchestration become complex enough to justify Logic Driver Pro?

These experiments are more valuable than buying another plugin without a concrete requirement.

---

# Decision Rule

For every system in the laboratory:

1. Identify the actual uncertainty or requirement.
2. Use Unreal-native functionality if it solves the problem well enough.
3. Evaluate mature existing plugins.
4. Integrate or extend rather than duplicate when possible.
5. Use Logic Driver only when visual orchestration / authoring provides measurable value.
6. Promote reusable capabilities into BertaDevKit only after they prove useful in real experiments.
7. Keep product-specific narrative/content logic in the product repository.
8. Treat discounts as an opportunity to acquire a justified capability, not as justification by themselves.

The desired outcome is a reinforcing loop:

**Personal experiment → technical learning → reusable tooling → stronger professional R&D capability.**
