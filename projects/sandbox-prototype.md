# Sandbox Prototype

Unreal Engine 5.8 integration sandbox used to install, configure, and validate reusable plugins before adopting them in a concrete product or prototype.

The goal is to reduce integration risk, understand dependencies and interoperability, and preserve a known-good reference setup without coupling plugin experimentation to the Biblical Sanctuary project or another product.

---

## Rebuild 2026-10-19 — plugin inventory and verification

A fresh **UE 5.8 Blank C++** Sandbox was created after the GAS Companion Third Person template failed to create successfully. Plan: add the stock Unreal **Third Person** Feature Pack (not GAS Companion's template), then integrate and test plugins incrementally with restore points. **Moonville is postponed until the other selected plugins are validated.** The user subsequently reported that all 21 non-Moonville items were added to the new Sandbox, but their runtime/packaging validation remains unverified.

### Official documentation for every plugin

**Before researching or editing any integration, consult [the central documentation index for all 27 plugins](../research/plugins/plugin-documentation-index.md).** It lists the manufacturer's technical manual when available, source repositories, Fab/product documentation, access limitations, the four Moonville Google Docs and vendor-version caveats. **Do not ask the user to hunt for properties until the version-appropriate official guide has been checked.**

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

### Third Person baseline and environment test — 2026-10-19

**Stable baseline**

- User imported the stock Unreal **Third Person Blueprint Feature Pack** into the new **UE 5.8 Blank C++** Sandbox.
- Third Person gameplay passed in the Editor and in a **Windows packaged executable** (user-confirmed; exact package configuration not recorded).
- The untouched **full-project backup `01_ThirdPerson_Baseline` was completed before environmental edits** (user-confirmed).

**Environment scene — implemented in the Editor**

- Created `L_EnvironmentTest` as a duplicate of `ThirdPersonMap` (the original was preserved).
- Removed the original `DirectionalLight`, `SkyLight`, `SkyAtmosphere`, `ExponentialHeightFog`, and `VolumetricCloud` actors in the duplicate only.
- Added the `Ultra_Dynamic_Sky` actor; the sky, clouds and lighting were confirmed working.
- Confirmed its existing fog component is named `Height Fog`; `Use Volumetric Fog` was already enabled.
- Indoor-fog test: `Fog = 1.0`, both `Min Fog Start Distance` and `Max Fog Start Distance = 0`, then doubled `Base Fog Density` from the prior project value. **The density change made indoor fog clearly visible.** Exact before/after density values were not recorded.
- `r.SSFS 0` / `r.SSFS 1` visibly changed the scattering in the Editor; the effect was subtle with the initial thin fog, and **perceptible after increasing UDS fog density**.
- **Packaging update (user report 2026-10-19):** after the UDS/SSFS adjustments, the user confirmed the **new build worked perfectly**. This is a **successful environment-stage packaged-build checkpoint by user report**, separate from the earlier Third Person baseline. The exact build configuration, saved Game Default Map, and independent A/B test of SSFS inside the packaged executable were **not separately reported**.

**Documentation — check these sources first when continuing**

- [Ultra Dynamic Sky — our exact Sandbox setup and test notes](../research/plugins/ultra-dynamic-sky.md)
  - [Official UDS documentation index by release](https://www.ultradynamicsky.com/)
  - [UDS 9.4 manual used for this research](https://www.ultradynamicsky.com/Documentation/V9/9-4)
  - The **actual installed UDS version remains unknown**; use the in-editor **UDS Documentation** button to select the version-matched manual before making further changes.
- [Screen Space Fog Scattering — our Sandbox findings and CVars](../research/plugins/screen-space-fog-scattering.md)
  - [Official SSFS documentation](https://dmkarpukhin.com/docs/ssfs/)
  - Documentation shows plugin version 1.32 for UE 5.8; the locally installed plugin version is unverified.

**Packaging gate — user-confirmed success (2026-10-19)**

- The user confirmed **the new environment-stage build worked perfectly**. Record as **packaged-build checkpoint passed**; do not conflate with the earlier baseline build.
- `Game Default Map = L_EnvironmentTest` was requested in Project Settings → Maps & Modes. The precise saved setting and SSFS console A/B in the executable have not been separately confirmed.
- **Backup completed (user-confirmed 2026-10-19):** full-project restore point `02_Environment_UDS_SSFS` was created after the successful environment-stage build. The exact folder name was proposed; user confirmed completion in immediate response. **Validated environment checkpoint is closed.** Proceed with one candidate at a time, preserving `L_EnvironmentTest`.
- **Next active candidate (user decision 2026-10-19):** Chameleon Post Process in a duplicate named `L_ChameleonTest`, one effect at a time, preserving UDS + SSFS. GAS Companion gameplay foundation, Nice Interaction, cameras and Moonville are deferred.

**Preferred workflow:** Integrate one change → playtest → package at checkpoints → document confirmed outcome → backup → next change. Separate official documentation from project-specific observations, and do not guess exact Blueprint properties or numeric settings.

### Next active experiment — Chameleon Post Process (2026-10-19)

**User decision:** continue with the visual stack first. Chameleon is already installed in the UE 5.8 Sandbox. **Do Chameleon before starting the GAS Companion character foundation, Nice Interaction System V2 or camera plugins.** GAS is postponed, not rejected.

**Isolation plan:** duplicate the validated `L_EnvironmentTest` level as `L_ChameleonTest`, keeping `L_EnvironmentTest` and backup `02_Environment_UDS_SSFS` untouched. Verify the duplicated level opens and plays with UDS + SSFS intact. Then, **one step at a time**, follow official [SUMFX Chameleon Getting Started](https://sumfx.net/docs/chameleon/general-chameleon/getting-started/): locate `Chameleon/Chameleon` Blueprint in Content Browser, drag the Chameleon actor into the test level, and use its actor Details to toggle **one** post-process effect. By default the actor is global (`Chameleon Settings → Unbound`). **Confirmed 2026-10-19:** user duplicated/opened `L_ChameleonTest`, placed `Chameleon` actor and successfully enabled **Invert** in viewport. User subsequently confirmed Invert also **works in Play-in-Editor while controlling the character**. Chameleon basic visual/runtime smoke test passed in Editor. **Integration A/B passed (user-confirmed 2026-10-19):** with Chameleon `Invert` active in Play-in-Editor, switching `r.SSFS 0`/`r.SSFS 1` still produces a **perceptible difference**; the underlying fog remained visible. Therefore the **Chameleon + UDS + SSFS visual integration is validated in Editor/PIE**. **Packaged-build milestone passed (user-confirmed 2026-10-19):** user set `Game Default Map = L_ChameleonTest`, completed packaging and confirmed the build runs correctly with the Chameleon test setup. This confirms integration through the packaged build; a specific `r.SSFS` A/B inside the packaged game, performance and cross-platform behavior have not been independently checked. **Next:** full-project restore-point backup suggested `03_Chameleon_Validated` (not yet confirmed created).

**Next gameplay milestone — revised user preference 2026-10-19:** integrate GAS Companion by **reparenting the existing stock Third Person Character Blueprint itself** to `GSCModularCharacter`, rather than duplicating the Character. GAS Companion [official Quick Start](https://gascompanion.github.io/quick-start/) explicitly supports reparenting `ThirdPersonCharacter` directly. This retains the existing asset references and avoids an extra GameMode/Character copy. **Trade-off:** all levels using the original Character are affected. **Safety gate:** confirm a full backup of the current working project before reparenting; **`03_Chameleon_Validated` full-project backup completed (user-confirmed 2026-10-19).** **Reparent confirmed by user (2026-10-19):** the original `BP_ThirdPersonCharacter` was reparented to `GSCModularCharacter`, compiled without errors, and `GSCAbilitySystemComponent` appeared in the Components panel. **Next validation:** Play-in-Editor and verify Move, Look and Jump still work; gameplay/PIE has **not yet been confirmed** after reparenting. Do not configure new abilities until this check passes. Keep native Move/Look; later implement Jump or Sprint as abilities, then attributes if useful. After this milestone, evaluate Nice Interaction / one camera plugin.

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
