# Biblical Sanctuary Prototype

## Purpose

A small first-person biblical storytelling prototype intended to serve simultaneously as:

- a short, complete interactive experience;
- a practical integration test for the current Unreal R&D plugin stack;
- a public R&D / prototyping case study;
- a possible first Steam release in the biblical storytelling niche.

The target is deliberately small: approximately **45–60 minutes**, with a strong bias toward reuse, simple mechanics, atmosphere, exploration, and learning rather than large-scale game production.

This should remain an R&D laboratory, not become a multi-year indie-game project.

---

## Existing opportunity

A set of previously created Sanctuary / Tabernacle assets is available, including assets representing items such as:

- courtyard curtains;
- altar of sacrifice;
- laver;
- tabernacle / tent and coverings;
- altar of incense;
- table of the bread of the Presence;
- menorah;
- veil between the Holy Place and Most Holy Place;
- Ark of the Covenant.

### Publication risk

Before any public or commercial release, verify that these assets have clear permission for commercial use and redistribution inside a shipped game.

Being given the files in the past does **not automatically prove** that commercial publication / redistribution rights were transferred. Obtain or recover written permission if possible before treating the assets as production-safe.

---

# Core concept

The player explores a compact desert environment surrounding an initially incomplete Sanctuary reconstruction.

Across several distinct locations, the player discovers small representations / clues corresponding to major Sanctuary elements.

Core loop:

```text
Explore a distinct area
        ↓
Discover an object / clue
        ↓
Interact using Nice Interaction
        ↓
Inspect it using Inspection System Pro
        ↓
Learn its purpose / biblical context
        ↓
Register discovery / narrative state
        ↓
Corresponding Sanctuary element becomes reconstructed
        ↓
Next search area / clue becomes available
```

The strongest part of the concept is the visible reconstruction of the Sanctuary as a persistent representation of player progress.

The educational information and the gameplay layer should remain clearly distinguishable: the biblical / historical description should be treated carefully, while the treasure-hunt / miniature-object mechanic is an explicit game abstraction rather than something presented as a biblical event.

---

# Recommended scope

Do **not** turn every individual Sanctuary component into a separate collectible.

Target approximately **6–8 meaningful reconstruction stages**.

Possible grouping:

1. **Courtyard / entrance**
2. **Altar of sacrifice**
3. **Laver**
4. **Tabernacle structure and coverings**
5. **Table + menorah**
6. **Altar of incense + veil**
7. **Ark of the Covenant / Most Holy Place**

Exact theological / historical ordering and wording should be researched separately before production. The sequence above is a gameplay structuring proposal, not a claim that it represents the canonical construction sequence.

Target pacing:

- 5–7 minutes per stage;
- approximately 45–60 minutes total;
- no filler required to reach an arbitrary duration.

---

# World design

Avoid a large empty desert.

Use a **compact hub-and-spoke environment** with the Sanctuary near the center and several recognizable exploration landmarks around it.

Possible landmarks:

- rocky ridge;
- dry riverbed / wadi;
- small camp;
- lone acacia / vegetation cluster;
- ruined or temporary shelter;
- cave / rock recess;
- hill or overlook.

The Sanctuary should remain visible from many areas when possible. As it reconstructs, the changing silhouette becomes both navigation aid and reward.

PCG is useful for fast environmental dressing, but authored landmarks should carry navigation and gameplay meaning. PCG should fill space, not replace level design.

---

# Guidance / minimap strategy

Do not show every exact collectible location immediately; that converts exploration into walking between icons.

Use progressive assistance:

```text
Stage begins
   ↓
show broad search zone / narrative clue
   ↓
player explores freely
   ↓
if player is stuck for a while
   ↓
stronger hint
   ↓
finally reveal precise minimap marker
```

This makes the Map / Minimap system useful without eliminating discovery.

Suggested hint escalation:

1. textual / environmental clue;
2. broad map region;
3. stronger directional hint;
4. exact icon after a configurable timeout.

The timeout should be tuned through playtesting rather than fixed arbitrarily.

---

# Avoid repetitive collectible design

The main design risk is repeating:

> walk → find miniature → read text → repeat.

Each discovery should have a small variation while preserving a cheap implementation model.

Examples:

- one object is visible but requires finding a path to reach it;
- one requires inspecting environmental clues first;
- one is found inside a small shelter;
- one becomes available after another reconstruction stage;
- one involves choosing between two paths;
- one uses audio / visual guidance rather than a marker.

Do not introduce complex combat, crafting, inventories, AI enemies, or traversal systems merely to add variety.

---

# Sanctuary reconstruction

The reconstruction is the principal progression reward.

Possible implementation tiers:

### Tier 1 — simplest
Toggle pre-placed Sanctuary components visible when their discovery state becomes true.

### Tier 2 — recommended
Use a short polished reveal per component:

- fade / dissolve;
- dust / subtle Niagara;
- audio sting;
- camera emphasis only when appropriate.

### Tier 3 — avoid initially
Fully animated physical construction sequence for every component.

That adds substantial animation / sequencing work without proportionate R&D value.

The visual treatment should fit the intended theological tone. Avoid making sacred objects feel like arcade loot drops.

---

# Candidate plugin stack

Use only systems that solve a real need.

## High-value baseline

- **Menu System Pro / CommonUI** — menus, settings, controller-first UI.
- **Nice Interaction System V2** — world interaction.
- **Inspection System Pro** — inspect discoveries and present explanatory content.
- **Narrative Tales** — progression, narrative state, contextual text / dialogue where useful.
- **Map / Minimap / Markers** — progressive hinting and orientation.
- **Ultra Dynamic Sky** — desert lighting, time, sky, weather / atmosphere.
- **Chameleon** — restrained color grading / perception effects if they improve presentation.
- **Screen Space Fog Scattering** — only if the chosen atmospheric treatment actually uses fog / haze appropriately.
- **MetaSounds / Unreal Audio** — environmental atmosphere and discovery feedback.

## Conditional

- **PCG / PCGX** — desert dressing and biome scattering.
- **Save system** — resume progress if required; native / Menu System baseline first.
- **SteamCore Pro** — later, for Steam achievements / platform integration when purchased.
- **GAS Companion** — only if a real gameplay ability requirement emerges; not required by the current concept.
- **Inventory System Pro** — probably unnecessary if discoveries are educational/progression records rather than gameplay inventory.

---

# First-person presentation

Prefer **first person without visible arms/hands** for the initial prototype.

Benefits:

- avoids hand / arm animation production;
- avoids IK and camera clipping work;
- simplifies interactions;
- keeps attention on environment, inspection, narrative and atmosphere;
- reduces the amount of bespoke content required.

Add visible hands later only if a specific interaction materially benefits from them.

---

# Suggested vertical slice

Before building the whole experience, validate one complete loop with only:

- central incomplete Sanctuary;
- one small desert search area;
- one discoverable object;
- Nice Interaction;
- Inspection System presentation;
- one narrative / progression state;
- one minimap hint escalation;
- one Sanctuary component reconstruction.

Success criterion:

> Does discovering, learning about, and then seeing the Sanctuary physically change feel satisfying enough to repeat six or seven times?

If the answer is no, change the core loop before adding content.

---

# R&D value

This prototype is particularly useful because it forces integration across several existing tools without requiring a large amount of bespoke gameplay code.

It can validate:

- plugin interoperability;
- first-person interaction architecture;
- CommonUI / controller-first navigation;
- narrative progression state;
- inspection / educational content delivery;
- adaptive map guidance;
- PCG-assisted environment creation;
- atmosphere and post-processing;
- save / resume flow;
- later Steam integration and Steam Deck testing.

Potential reusable outputs should be evaluated for BertaDevKit only when repeated integration friction is observed. Do not build adapters speculatively.

---

# Current recommendation

Proceed with this as a **candidate first biblical storytelling prototype**, but validate the single-discovery vertical slice before committing to the complete Sanctuary.

The concept should optimize for:

**small → polished → educational → atmospheric → reusable → finishable**

rather than maximizing mechanics or content volume.
