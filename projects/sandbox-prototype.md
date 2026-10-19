# Sandbox Prototype

Unreal Engine 5.8 integration sandbox used to install, configure, and validate reusable plugins before adopting them in a concrete product or prototype.

The goal is to reduce integration risk, understand dependencies and interoperability, and preserve a known-good reference setup without coupling plugin experimentation to the Biblical Sanctuary project or another product.

---

## Rebuild 2026-10-19 — plugin inventory and verification

A fresh **UE 5.8 Blank C++** Sandbox was created after the GAS Companion Third Person template failed to create successfully. Plan: add the stock Unreal **Third Person** Feature Pack (not GAS Companion's template), then integrate and test plugins incrementally with restore points. **Moonville is postponed until the other selected plugins are validated.** The user subsequently reported that all 21 non-Moonville items were added to the new Sandbox, but their runtime/packaging validation remains unverified.

### User-reported plugins available to consider

The user named or confirmed the following plugins on 2026-10-19. **This is the full reported inventory, not proof that every item is enabled or working in the new Sandbox**. Keep the reported names rather than silently equating similar products.

1. Animation Tools Bundle
2. Dragon IK
3. Runtime Audio Importer
4. Black Eye
5. Ultimate Gameplay Camera
6. Auto Size Comments
7. Blueprint Assist
8. Electronic Nodes *(confirmed in follow-up; earlier voice transcription appeared as "Electronic Notes")*
9. Global Search Pro
10. Graph Printer
11. Combo Graph
12. GAS Companion
13. Global Variable System
14. Low Entry Extended Standard Library
15. Narrative Tales
16. State Tree Tools
17. PCGEx (PCG Extended Toolkit) *(confirmed in follow-up; earlier referred to as PCG Extension)*
18. Screen Space Fog Scattering
19. HTTP JSON and WebSocket Blueprint Plugin *(user identifies this as the plugin previously called 'UForge' in discussion; verify exact asset name in the editor)*
20. BertaDevKit
21. Nice Interaction System V2 *(not a Moonville product)*
22. Chameleon
23. Ultra Dynamic Sky

BertaDevKit was previously confirmed in the former Sandbox. Nice Interaction System V2 was present on the 21-item working list and is separate from Moonville; it is now explicitly included in the numbered inventory. Chameleon and Ultra Dynamic Sky were added following the user's correction, and on 2026-10-19 the user confirmed both are installed in the new Sandbox; runtime and packaging validation are still pending. The four Moonville systems remain on the future integration list: Menu System Pro V2, Inventory System Pro, Inspection System Pro and Map System Pro. **2026-10-19 confirmation:** the user reports that **all four are still missing/not yet added to the new Blank C++ Sandbox**. This is distinct from their previously documented state in the former Sandbox below.

**Important naming distinction:** the vendor plugin name reported above, **HTTP JSON and WebSocket Blueprint Plugin**, must not automatically be conflated with Unreal's **HTTP, JSON and WebSocket Networking** prerequisite or with JSON Blueprint Utilities. Check each actual plugin descriptor before recording equivalence.

### Current reported installation state — 2026-10-19

The user confirms that **all original 21 items on the working list** (the 19 originally reported third-party/tools plus **BertaDevKit** and **Nice Interaction System V2**) have been **added to the new UE 5.8 Blank C++ Sandbox**. The additional plugins **Chameleon** and **Ultra Dynamic Sky** were also confirmed installed by the user on 2026-10-19. **All 23 non-Moonville items** in the reported inventory are now installed in the new Sandbox; individual runtime and packaging validation remain unverified.

**Do not conflate added/installed with runtime validation:** compilation, PIE behavior, editor restart, inter-plugin compatibility, packaging and restore-point creation are not yet independently confirmed for this rebuilt Sandbox.

**Still not added:** all four Moonville systems — Menu System Pro V2, Inventory System Pro, Inspection System Pro, and Map System Pro. Keep the planned integration order one-at-a-time after confirming a usable current baseline.

### Third Person and environment test — 2026-10-19

- User added Unreal's standard **Third Person Blueprint Feature Pack** to the new **Blank C++** Sandbox; the Third Person gameplay works in Editor (user-confirmed).
- **Windows packaged build completed successfully and the packaged game ran correctly outside the Editor** (user-confirmed 2026-10-19). Build configuration unspecified; do not infer Development/Shipping.
- **Full-project restore-point backup `01_ThirdPerson_Baseline` completed** (user-confirmed 2026-10-19), after successful packaged build and before sky/fog modifications.
- Next environment experiment: duplicate `ThirdPersonMap` to `L_EnvironmentTest` (proposed name, not yet confirmed created).
- **Ultra Dynamic Sky initial integration confirmed working in Editor (2026-10-19):** duplicated `ThirdPersonMap` as `L_EnvironmentTest`, removed the existing DirectionalLight, SkyLight, SkyAtmosphere, ExponentialHeightFog and VolumetricCloud actors from the duplicate, then added `Ultra_Dynamic_Sky`. User confirmed sky/lighting functional; no environment-specific packaged-build test yet.
- **Screen Space Fog Scattering next:** inspect whether `Ultra_Dynamic_Sky` contains an `ExponentialHeightFog` component before tuning the fog. Once verified, test SSFS by A/B comparison with `r.SSFS 0` and `r.SSFS 1`. Do not add a duplicate fog actor without evidence of need.
- References: https://www.ultradynamicsky.com/ and https://dmkarpukhin.com/docs/ssfs/ ; internal notes: [Screen Space Fog Scattering](../research/plugins/screen-space-fog-scattering.md).

### Suggested progress states

Track each plugin separately as **available / installed / enabled / smoke-tested / packaged-tested**, and record test date, version, integrations changed, issues and restore-point name. Do not mark a test passed merely because the editor opens.

---

## Previous Sandbox baseline (historical)

- Unreal Engine 5.8.
- First-person playable baseline is working.
- Character, GameMode, and PlayerController remain Blueprint-based while the goal is plugin integration and validation. Move logic to C++ only when there is a concrete architectural or reuse benefit.
- BertaDevKit is integrated.
- Third-party plugins are being integrated and validated incrementally.

---

## Moonville stack (previous Sandbox; historical state)

Detailed reproducible setup, decisions, validation results, troubleshooting notes, and current integration progress are maintained in:

**[Moonville Stack Integration — UE 5.8 Sandbox](../research/plugins/moonville-stack-integration.md)**

Status reached in the **previous** Sandbox before restarting:

- **Menu System Pro V2 Common UI** — integrated and fully validated, including Windows Development packaged-build testing.
- **Inventory System Pro** — present; combined Menu + Inventory + Inspection integration in progress.
- **Inspection System Pro** — present; combined Menu + Inventory + Inspection integration in progress.
- **Map System Pro** — present; dedicated integration not started yet.

The previous integration strategy was to use Menu System Pro as the shared Common UI / Enhanced Input foundation and integrate Inventory and Inspection against that known-good baseline before moving to Map System Pro.

### Required Unreal plugins

For the current Moonville setup:

- HTTP, JSON and WebSocket Networking
- JSON Blueprint Utilities
- Common UI
- Enhanced Input

`HTTP, JSON and WebSocket Networking` is one Unreal plugin. `Enhanced Input` is required by the stack and is already available/enabled in the current Unreal Engine 5.8 baseline.

---

## Menu System Pro — deferred product customization

A simplified player-facing configuration has been discussed for future concrete products:

- Display Mode
- Resolution
- Quality Preset (Low / Medium / High / Epic)
- remove individual graphics controls
- English only while no other language is supported
- remove Moonville branding
- keep Unreal branding
- remove game-name text from the Menu System UI
- use a black background until final key art exists

These changes are **not part of the current Sandbox baseline**. The Sandbox currently keeps the vendor base substantially intact so it remains useful as a reference integration project.

---

## Validation rule

Do not treat a plugin as fully integrated merely because it compiles or opens in the editor.

For each system, verify its minimum runtime workflow and record any project settings, input mappings, components, interfaces, cross-plugin assumptions, troubleshooting results, and packaged-build requirements needed to reproduce the setup.

The detailed Moonville record should be updated at each validated milestone so integration knowledge does not remain only in chat history.
