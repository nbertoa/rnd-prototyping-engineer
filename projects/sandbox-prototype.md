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

### Blue Dawn sky approved visually — 2026-10-27

**New look:** `L_Sky_BlueDawn`, early blue-hour/dawn (cool blue/violet sky), proposed 05:40 time with Volumetric Clouds. The user confirmed that increasing **`Lighting Brightness → Dawn/Dusk` from `0.65` to `0.85`** delivered the desired brighter appearance and that the look is satisfactory. **All proposed color/lighting/movement values and their evidence status are recorded in [Blue Dawn sky preset](uds-blue-dawn.md).** This is a **user-approved Editor visual outcome**, not a verified UDS config asset, project-file inspection, performance test, or packaged build.

### Ultra Dynamic Sky visual showcase — planned 2026-10-26

**Goal:** create two separate, static sky-focused Unreal maps from the preserved `L_EnvironmentTest` baseline: **`L_Sky_Sunset`** (dramatic warm sunset and large volumetric cloud forms) and **`L_Sky_ClearNight`** (clear starry night and restrained moon illumination). Keep `L_EnvironmentTest` and `L_ChameleonTest` untouched and do not implement a new day/night manager.

**Plan and acceptance gates:** [UDS Sunset + Clear Night](uds-sunset-clear-night.md). **Plugin reference:** [Ultra Dynamic Sky notes](../research/plugins/ultra-dynamic-sky.md).

**State:** two-level plan **revised 2026-10-26 after reviewing entire uploaded UDS 9.7 PDF (139 pages)**: [complete manual reference](../research/plugins/ultra-dynamic-sky-9-7-complete-reference.md). **Unreal levels not yet created/tested**; installed asset version still to be confirmed. Prior preliminary 9.4-based advice is superseded for this visual experiment.

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
  - [9.7 PDF fully reviewed — annotated technical reference](../research/plugins/ultra-dynamic-sky-9-7-complete-reference.md) (previous 9.4 online manual was used before receiving the PDF)
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

**Next gameplay milestone — revised user preference 2026-10-19:** integrate GAS Companion by **reparenting the existing stock Third Person Character Blueprint itself** to `GSCModularCharacter`, rather than duplicating the Character. GAS Companion [official Quick Start](https://gascompanion.github.io/quick-start/) explicitly supports reparenting `ThirdPersonCharacter` directly. This retains the existing asset references and avoids an extra GameMode/Character copy. **Trade-off:** all levels using the original Character are affected. **Safety gate:** confirm a full backup of the current working project before reparenting; **`03_Chameleon_Validated` full-project backup completed (user-confirmed 2026-10-19).** **Reparent confirmed by user (2026-10-19):** the original `BP_ThirdPersonCharacter` was reparented to `GSCModularCharacter`, compiled without errors, and `GSCAbilitySystemComponent` appeared in the Components panel. **PIE validation PASSED (user-confirmed 2026-10-19):** after reparenting the original `BP_ThirdPersonCharacter` to `GSCModularCharacter`, **Move, Look and Jump all continue working** in `L_EnvironmentTest`. The existing Third Person inputs remain native; no Gameplay Abilities have been granted or exercised yet. **2026-10-19: first Gameplay Ability created (user-confirmed):** created `GA_Jump`, parent `GSCGameplayAbility`, under project-owned `Gameplay/Abilities`, and it compiles. **Next:** implement `ActivateAbility` with `CommitAbility`, get avatar character, call native `Jump`, then `EndAbility`. Keep existing jump input unchanged until ability granted and activation separately validated. Packaging of this GAS-modified build and a post-GAS backup are NOT yet tested. Keep native Move/Look; later implement Jump or Sprint as abilities, then attributes if useful. After this milestone, evaluate Nice Interaction / one camera plugin.

### GAS Companion shared Gameplay Tag catalog — 2026-10-19

**Repository-verified update:** `nbertoa/ue5-bertadevkit` commit `ba0baf23c041df5f1c6789419642399ab1378f3f` introduced **474 explicit namespaced tags in 23 families** through `BertaGASCompanionExt/Config/Tags/*.ini`. Catalog [index](https://github.com/nbertoa/ue5-bertadevkit/blob/main/docs/bertagascompanionext/gameplay-tags/index.md), [GAS integration guide](https://github.com/nbertoa/ue5-bertadevkit/blob/main/docs/bertagascompanionext/gameplay-tags/integration.md), [lifecycle/ownership](https://github.com/nbertoa/ue5-bertadevkit/blob/main/docs/bertagascompanionext/gameplay-tags/lifecycle.md). The catalog defines labels only; it grants no abilities, actor tags, inputs, effects or logic. The specific installed Sandbox copy **has not yet been verified as updated**; check picker registration after plugin update/restart and `Import Tags From Config`.

**For Sandbox `GA_Jump` (not yet confirmed assigned):**

- **Asset Tags (Default AbilityTags)**: `Berta.Ability.Movement.Jump` — stable identity of the ability, selected in `GA_Jump` Class Defaults. Do not substitute old non-namespaced `Ability.Movement.Jump`.
- `Berta.State.Movement.Jumping` and `Berta.State.Movement.InAir`: **not** added to the ability's Activation Owned Tags by default. Derive these current conditions from Character Movement with a single authoritative owner and explicit cleanup; ending GA immediately after `Jump()` makes activation-owned tags too short lived.
- `Berta.Event.Movement.Jumped` and `Berta.Event.Movement.Landed`: optional occurrence events emitted at accepted takeoff and confirmed landing by their producer, not persistent actor/ability identity tags.
- `Berta.Input.Movement.Jump`: optional semantic label for a project's input router, **not** a substitute for assigning the real `IA_Jump` Input Action in GAS Companion.
- `Berta.Cooldown.Movement.Jump`, `GameplayCue.Berta.Movement.Jump`: defined but not needed for the basic jump prototype. A registered cooldown tag grants no timer; cues require explicitly configured handlers.
- Optional **Activation Blocked Tags** like `Berta.State.Control.Stunned` are project design policies and should be adopted only after that condition is actually maintained. No arbitrary blocked tags yet.

**Gameplay tag visibility issue (user-reported resolved, 2026-10-19):** in the Sandbox, plugin source INIs were visible in the source picker but the tag tree did not show the names even after full C++ rebuild and config import was enabled. The user reported resolution after a local test changing the leading `+GameplayTagList` to `GameplayTagList` on the jump tag row in the installed plugin's `BertaAbilities.ini`. **Scope of repair NOT verified:** whether only `Berta.Ability.Movement.Jump` or all 474 tags are visible. At the time checked, BertaDevKit GitHub `main` still contains `+GameplayTagList` for the jump row; this should be investigated and corrected through the BertaDevKit project's own change/validation workflow, not patched repeatedly in Sandbox copies. **Next step:** in `GA_Jump` set **Asset Tags (Default AbilityTags)** to `Berta.Ability.Movement.Jump`, compile and save; leave Activation Owned Tags empty until its state owner and lifecycle are implemented. Do not modify `IA_Jump` input wiring before the new ability is granted/tested.

### GAS Companion ability grant and input binding — 2026-10-19

**User-confirmed:** `GA_Jump` has been assigned `Berta.Ability.Movement.Jump` as its Asset Tag; the user reports that adding it to the original `BP_ThirdPersonCharacter`'s `GSCAbilitySystemComponent → Granted Abilities` works. **Not yet confirmed:** that input activation of the new Gameplay Ability has replaced the original Third Person Jump event, nor that an actual ASC activation was observed.

**Next one step:** add `GSCAbilityInputBinding` component to existing `BP_ThirdPersonCharacter`; leave its optional `Input Mapping Context` empty because the Third Person template already registers `IMC_Default` via Enhanced Input, to avoid redundant context additions. Compile/save, verify presence before altering jump input.

**User confirmed 2026-10-19:** `GA_Jump` with `Wait Input Release` → `Stop Jumping` → `End Ability` is working; earlier reported failure was user setup error, not a verified system fault. The user has been following GSC Ability Input Binding, Modular PlayerController and `IA_Jump` binding walkthrough, but *do not claim independent packaging/network checks*. Next separately validate landing notifications (`BP_ThirdPersonCharacter → Event OnLanded`), then decide whether to send `Berta.Event.Movement.Landed` only if there is a listener/consumer. The authoritative source for `Berta.State.Movement.InAir`/`Jumping` remains CharacterMovement and need not share the ability's lifetime. Avoid unnecessary longer-running ability just to model airborne state.

### CharacterMovement-derived airborne state — 2026-10-19

**User-confirmed runtime PIE:** `BP_ThirdPersonCharacter → Event On Movement Mode Changed → Branch(Is Falling)` now correctly adds `Berta.State.Movement.InAir` as an ASC loose tag on True and removes it on False. An earlier report that the tag remained after landing was due to the False execution pin not being connected; user fixed it and reports correct behavior. The native `OnLanded`-based removal workaround is **not needed** and should not coexist as a second removal route. Only local PIE behavior confirmed; initial state on possession, replication, non-Falling airborne modes, and packaging have not been validated.

**PIE integration tests PASSED (user-confirmed 2026-10-19):** all five validation cases passed: ordinary jump activates both `Berta.State.Movement.InAir` and `Berta.State.Movement.Jumping`; early Space release keeps both movement states active while airborne; landing removes both; walking off a platform activates `InAir` but not `Jumping`; repeated valid jumps continue to activate `GA_Jump`. The configured source of truth is native CharacterMovement; `Jumping` starts on the native `Event On Jumped` and clears along with `InAir` when falling mode ends. `Wait Input Release` ends ability input behavior separately. **Next gate:** Windows Development packaged executable with moving, looking, jumping, and repeated jumps. **NOT YET user-confirmed:** packaging and full-project restore point `04_GASCompanion_Jump_Validated`; create backup only after packaging passes. Gameplay Event `Berta.Event.Movement.Landed` is deliberately deferred until a consumer is identified.

### GA_Jump airborne repeat-activation gate — 2026-10-19

**New user-reported observation:** pressing Jump again midair still attempts to activate `GA_Jump` even though the normal movement has been working; not yet verified whether the Character actually gains an extra jump or simply reattempts GAS activation. PIE movement-state tests passed earlier and `Berta.State.Movement.InAir` is maintained on the character ASC as a loose Gameplay Tag through `On Movement Mode Changed`.

**Fix PASSED in PIE (user-confirmed 2026-10-19):** in `GA_Jump` Class Defaults, **Activation Blocked Tags** = `Berta.State.Movement.InAir` prevents the undesired midair reactivation; the user confirmed the behavior now works. GAS natively refuses to activate an ability if its ASC owner has any Activation Blocked Tags (UE 5.8 Gameplay Abilities documentation). This covers deliberate jumping and accidental walk-off; `Berta.State.Movement.Jumping` alone would not cover walk-off. Do not add `Berta.State.Movement.InAir` to ability Activation Owned Tags or alter current native movement synchronization. Keep `Berta.Ability.Movement.Jump` as Asset Tags. Midair jump reactivation is user-confirmed fixed; full separate regression details were not enumerated. Before closing the GAS Jump milestone, validate a Windows Development packaged executable with movement/look/jump and repeated jumping. Existing `Wait Input Release` behavior remains. No new packaging/backup has been confirmed.

### GAS Companion stamina cost + built-in minimalist HUD — 2026-10-19

**User preference update:** do **not** add a cooldown to `GA_Jump` now. Instead, each valid jump should consume `GSCAttributeSet.Stamina`; show GAS Companion's existing minimalist Health/Mana/Stamina HUD (`WB_HUD_Minimalist`), then optionally add periodic regeneration. Do not build custom UI or create a jump cooldown GE for this milestone.

**Documented native path:** [GAS Companion Quick Start](https://gascompanion.github.io/quick-start/) states `GSCModularCharacter` does **not** automatically grant `GSCAttributeSet`. First add `GSCAttributeSet` in `BP_ThirdPersonCharacter → GSCAbilitySystemComponent → Granted Attributes`, with `AttributeMetaData` initialization table (`GSCAttributeSet.Health/MaxHealth, Stamina/MaxStamina, Mana/MaxMana`, values > 0). Then spawn existing `WB_HUD_Minimalist` (derived from `GSCUWHud`) with Create Widget + Add to Viewport at the correct local character/player lifecycle; see [Working With UI](https://gascompanion.github.io/working-with-ui/). Create `GE_Jump_Cost` Instant with Modifier `GSCAttributeSet.Stamina` Additive negative magnitude, set as `GA_Jump → Cost Gameplay Effect Class`. `GA_Jump`'s existing `Commit Ability` applies costs after checking availability. Verify insufficient stamina blocks ability activation and remains consistent with `Berta.State.Movement.InAir` activation blocking. Optionally add `GE_Stamina_Regen` Infinite periodic and grant via `GSCAbilitySystemComponent → Granted Effects`. **User-confirmed 2026-10-19:** `GSCAttributeSet` has been added to the character's Granted Attributes. **Next manual step:** create `/Game/Gameplay/Attributes/DT_Player_Attributes` with row structure `AttributeMetaData`, six rows `GSCAttributeSet.Health`, `.MaxHealth`, `.Mana`, `.MaxMana`, `.Stamina`, `.MaxStamina`, each Base Value 100 (example values), then save. **Next guidance, not yet user-verified:** after creating `DT_Player_Attributes`, bind it as `GSCAttributeSet` Initialization Data and check initialized Health/Mana/Stamina values in PIE. To expose the values, use GAS Companion's existing `WB_HUD_Minimalist` (`GSCUWHud` child) in the singleplayer `BP_ThirdPersonCharacter`: `BeginPlay → Create Widget (Class WB_HUD_Minimalist, Owning Player GetPlayerController(0)) → Add to Viewport`. Ensure GAS Companion plugin content is visible in Content Browser. Verify the three bars populate, *then* implement Stamina cost on jump via an Instant Gameplay Effect. Neither Data Table initialization nor HUD display is independently confirmed by the user's short 'seguí' responses.

**Next manually instructed GAS cost step (user confirmation pending):** create `GE_Jump_Cost` from `GA_Jump → Cost Gameplay Effect Class`, with Duration Policy `Instant`, Modifier target `GSCAttributeSet.Stamina`, operation `Add` and static magnitude `-40`. Set GA_Jump cost property to this GE, retain the existing `Commit Ability` path (which checks and applies costs); do not subtract Stamina manually in Blueprint. With initialized 100 Stamina and no regen, expect 100→60→20, then third activation fails cost check. Test visible HUD update and debug actual ASC attribute, inspect `bLooselyCheckAbilityCost` if cost eligibility unexpected. Reference: https://gascompanion.github.io/quick-start/#stamina-cost . **Next instructed step (NOT yet verified in PIE):** create `GE_Stamina_Regen`, Duration Policy `Infinite`, modifier on `GSCAttributeSet.Stamina` with `Add` +5, period 0.2 seconds, and assign it through the character `GSCAbilitySystemComponent → Granted Effects`. This follows the [official GAS Companion quick start, Stamina Regen Gameplay Effect](https://gascompanion.github.io/quick-start/#stamina-regen-gameplay-effect). Test that spending stamina through `GA_Jump` is followed by recovery and that values stop at `MaxStamina`. Do not add tag-based suppression of regen while airborne at this point. Costs, initial values, and UI behavior have not been independently confirmed by the user's `seguí` responses. Windows packaged build and `04_GASCompanion_Jump_Validated` backup remain pending.

### GAS Companion Stamina HUD — visual interpolation deferred (2026-10-19)

**User-confirmed:** Stamina regeneration is working and the existing stepped visual bar update is acceptable for this Sandbox iteration. **Decision:** do not implement visual interpolation/smoothing, do not modify GAS Companion's vendor `WB_HUD_Minimalist` to smooth it, and do not assume a custom duplicate widget was created. Continue using the current HUD as-is. Stamina-cost and HUD interaction details beyond regeneration were not separately revalidated in this turn. Windows Development packaging and restore point `04_GASCompanion_Jump_Validated` remain pending; do not mark complete until tested.

### Sprint ability (new milestone, started 2026-10-20)

The user asked to continue after the Windows Development packaging and `04_GASCompanion_Jump_Validated` backup instructions but did **not explicitly confirm either completed**. Keep both as pending verification; do not label them successful. The next feature is sustained sprint using GAS Companion and the existing `GSCAttributeSet.Stamina`, without custom movement framework.

**Next manual one-step task (creation not yet confirmed):** create `/Game/Gameplay/Abilities/GA_Sprint` deriving `GSCGameplayAbility`, set **Asset Tags** `Berta.Ability.Movement.Sprint`, **Activation Owned Tags** `Berta.State.Movement.Sprinting`, and `Instancing Policy = Instanced Per Actor`. Activation-owned Sprinting is appropriate if and only if sprint Ability lifetime matches accepted sprint motion; ensure robust `EndAbility`/cancellation and speed restoration. Reference: [Epic UE5.8 Gameplay Abilities](https://dev.epicgames.com/documentation/unreal-engine/using-gameplay-abilities-in-unreal-engine) and [GAS Companion quick start](https://gascompanion.github.io/quick-start/).

**Subsequent validations (not yet instructed):** configure `IA_Sprint` (Left Shift) in existing `IMC_Default`, add `GA_Sprint` to GSC Granted Abilities + Input Action, activate and maintain via `Wait Input Release`, change native `CharacterMovement.MaxWalkSpeed` for test, revert on normal end **and cancellation**, add stamina drain using a separate periodic Gameplay Effect only after baseline works, stop sprint automatically at zero stamina, verify `Berta.State.Movement.Sprinting` lifetime and packaged behavior. Prefer native CharacterMovement; consider effect-backed speed modifier as an architectural option later. Avoid claiming any new step implemented until user confirms.

### Sprint stamina drain — 2026-10-20

**User reported:** `IA_Sprint` is mapped to Left Shift and gamepad left-stick press (L3) in existing `IMC_Default`; `GA_Sprint` has been added to the existing Character; `Berta.State.Movement.InAir` blocks its activation. The user says the other Sprint setup is done; actual Sprint speed restoration/cancellation and packaging have not been independently verified. The missing requested behavior is stamina consumption while sprinting.

**Instructed configuration (pending user confirmation):** Create `GE_Sprint_Drain` using **Infinite**, **Period 0.2 seconds**, **Execute Periodic Effect on Application false**, and **Stamina Add -10 per tick**. Since `GE_Stamina_Regen` was configured earlier with **Stamina Add +5 per 0.2 seconds**, the expected *net change while sprinting* is `-5 / 0.2s = -25 Stamina/s`, provided both effects are active and the AttributeSet clamps Stamina correctly. In `GE_Sprint_Drain`, add native UE 5.8 GameplayEffect Component `Require Tags to Apply/Continue This Effect` with **Ongoing Tag Requirements → Require Tags** = `Berta.State.Movement.Sprinting` (do not use Application Tag Requirements; those would prevent the initial grant). Add `GE_Sprint_Drain` to the character's GSC `Granted Effects` **once**, analogous to the existing regen GE. It stays applied but inhibited outside sprint, avoiding manual apply/remove handles on ability end or cancellation. Epic reference: https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Plugins/GameplayAbilities/UTargetTagRequirementsGameplayEf- .

**Next separate validation:** test idle regen vs while-held stamina drain vs release; sprint effect pauses on release/cancel and re-enables when tag returns. **Important pending rule:** ending sprint and restoring speed when Stamina reaches 0 is NOT implemented by this effect; add a native WaitForAttributeChangeWithComparison threshold ability task or equivalent to cancel/end `GA_Sprint`, with one reliable speed reset for both release and cancellation and a guard against sprint beginning when empty. Do not claim zero-stamina stop or packaged behavior yet.

### Sprint start cost / insufficient-stamina activation — 2026-10-20

**Explanation to user:** `Commit Ability` performs native GAS cost and cooldown checks **only for configured ability costs**; the passive `GE_Sprint_Drain` assigned through character Granted Effects is independent and does not make `Commit Ability` reject zero Stamina. Reference: [GAS Companion Quick Start](https://gascompanion.github.io/quick-start/), [Epic Using Gameplay Abilities](https://dev.epicgames.com/documentation/unreal-engine/using-gameplay-abilities-in-unreal-engine), [GSC Loosely Check Ability Cost](https://gascompanion.github.io/ignore-ability-cost/).

**Proposed minimal gate (not yet user-confirmed):** create `GE_Sprint_StartCost` as **Instant** Stamina **Add -1**, assign to `GA_Sprint → Cost Gameplay Effect Class`, ensure `bLooselyCheckAbilityCost = false`, use existing `Commit Ability → Branch Return Value` and start sprint only on success; failure exits ability. This rejects activation below 1 Stamina but adds a **real 1-point upfront cost** on top of the periodic drain, a tradeoff clearly disclosed to the user. If zero startup cost is desired, use an explicit native GAS availability check/attribute-aware condition instead; do not invent a non-spending 'cost GE'. In either design, still implement **stop sprint and restore movement speed when Stamina reaches zero while active** (separate step). No implementation or runtime test confirmed yet.

### Sprint automatic termination at depleted stamina — 2026-10-20

**Current user request:** deactivate `GA_Sprint` automatically on stamina depletion. The previously proposed `GE_Sprint_StartCost` (`Instant` -1) blocks starting with insufficient stamina, but it does not monitor stamina after activation; periodic `GE_Sprint_Drain` still requires its own stop condition. StartCost creation itself has **not** been independently confirmed by user.

**Updated diagnosis 2026-10-20 (user reports threshold task triggers at every small Stamina change despite the advised settings):** no confirmed cause. Replace the direct termination wiring with a robust native GAS explicit value check: in `GA_Sprint` create Ability Task **Wait for Attribute Change** (not `Wait for Attribute Changed` Ability Async), Attribute `GSCAttributeSet.Stamina`, Trigger Once **false**, source tag filters empty; from its OnChange callback use **Get Ability System Component from Actor Info** → **Get Float Attribute from Ability System Component** (Stamina), require `Successfully Found Attribute` true, and **Branch (current Stamina <= 0)**. True → `End Ability`; False → do nothing, so listener remains active. For debugging temporarily Print String the returned *current Stamina* before the comparison, and check that the value actually decreases/does not go below zero as expected. **User-confirmed working 2026-10-20:** the explicit value guard fixes premature Sprint termination; `GA_Sprint` now ends on Stamina depletion instead of every small attribute change. Individual separate checks for restoring original MaxWalkSpeed on cancellation, repeated reactivation after regeneration, gamepad input, and packaged runtime remain to be verified. Reference: https://dev.epicgames.com/documentation/en-us/unreal-engine/BlueprintAPI/Ability/Tasks/WaitforAttributeChange and https://dev.epicgames.com/documentation/en-us/unreal-engine/BlueprintAPI/Ability/Attribute/GetFloatAttributefromAbilitySyst- . Run this task after successful Commit and sprint start; it listens only while the ability instance remains active. The existing `Wait Input Release` path separately ends the ability. Centralize `CharacterMovement.MaxWalkSpeed` restoration under inherited `Event OnEndAbility` (UE `UGameplayAbility::K2_OnEndAbility`), instead of relying only on the released-input path; cache original speed on activation before modifying it, to restore it correctly on normal completion and cancellation. Activation Owned Tag `Berta.State.Movement.Sprinting` is automatically removed at EndAbility, which should inhibit the granted tag-gated infinite periodic drain while idle. Verify the drain does not keep consuming and the actor is back at original movement speed when depleted and when released. A fresh activation should be blocked if stamina too low by the configured startup-cost check. Do not claim PIE or packaging passes until user verifies.

**Docs:** https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Plugins/GameplayAbilities/UAbilityTask_WaitAttributeChange-_1 ; https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Plugins/GameplayAbilities/UGameplayAbility .

### Sprint complete PIE behavior validation — 2026-10-20

**User-confirmed:** all four Sprint tests pass in PIE: (1) holding Shift/L3 raises movement speed and drains Stamina, (2) releasing input restores walking speed and regeneration resumes, (3) Stamina exhaustion ends Sprint and removes `Berta.State.Movement.Sprinting`, and (4) after Stamina recovers the ability can activate again. The final depleted-stamina listener uses `Wait for Attribute Change` with an explicit current Stamina `<= 0` check, rather than ending on any callback. This validates the desired Jump + Sprint + GAS Companion minimalist HUD workflows in PIE but **not** packaged runtime. Do not add cooldown or HUD interpolation; those were deferred.

**Next gate (pending):** create a fresh **Windows Development** package, using `L_EnvironmentTest` as the configured Game Default Map, and verify **outside Editor**: Move/Look, Jump with Stamina cost + InAir reactivation block, Sprint via Shift/L3 with speed gain, stamina drain and forced stop at zero, and the built-in Health/Mana/Stamina HUD with regen. Only after the executable passes, create a full-project restore point named `04_GASCompanion_JumpSprint_Validated` (supersedes the earlier proposed `04_GASCompanion_Jump_Validated` name, which was not confirmed created). Document failures before proceeding to other plugins.

### GAS milestone closed — Windows package and restore points confirmed (2026-10-20)

**User confirmed the Windows Development packaged executable works correctly** after Jump + Sprint + Stamina regen and the built-in GAS Companion HUD were integrated. This closes the GAS Companion current-feature **packaged-build validation**. Earlier full-project backup `04` was saved for **Jump**, and the user confirms the new full-project Jump + Sprint backup is numbered **`05`** (instead of the earlier proposed `04_GASCompanion_JumpSprint_Validated`). Keep the two independent restore points; exact suffix/spelling of the user's newly saved backup was not stated, so refer to it as **backup 05 (JumpSprint)** rather than claiming an exact folder name. Likewise backup 04 is an existing Jump checkpoint by user report. Prior sections labeled packaging or backup pending are superseded by this user confirmation.

**Next experiment:** Nice Interaction System **V2**, installed in the new UE 5.8 Sandbox but not yet smoke-tested. Vendor written guide: https://niceshadow.gitbook.io/nice-interaction-system/integration-in-your-project . Integrate incrementally in the current existing gameplay stack; **do not replace the GAS Companion Character or sacrifice working `GSCModularPlayerController`**. Minimal first setup step: `Project Settings → Engine → Collision → New Trace Channel`, name **`LookAtTrace`**, default response **`Ignore`**, as required by the vendor quick guide. Then validate `NiceInteractionSystem/Blueprints/Components/AC_PC_Interaction` has `LookAtTrace_Type` and `LookAtTrace_Collision` set to the created channel. Subsequent steps: add `AC_PC_Interaction` to the *existing* PlayerController class; if using the vendor's Blueprint component and stock `GSCModularPlayerController` native class, make a Blueprint subclass to attach it, update the project's GameMode; create/choose a GameState subclass and add `AC_GS_Interaction`, then test one interactable from vendor docs. Document any UE5.8/third-party conflicts. Only package and back up after a working minimal interaction loop is confirmed. This is a plan, not confirmed implementation.

### Nice Interaction V2 — actual PlayerController reuse (2026-10-20)

**User correction to proposed setup:** There is **already an existing Third Person PlayerController Blueprint** derived from `GSCModularPlayerController`. **Reuse that existing Blueprint**. Do **not** create an extra `BP_SandboxPlayerController` as earlier proposed, or change the GameMode's PlayerController class unnecessarily. Next single step: open the existing Third Person PlayerController Blueprint, add Nice Interaction component `AC_PC_Interaction` to its Components hierarchy, compile/save. Verify GameMode is using that existing controller only if interaction does not receive input, rather than assuming a new controller is needed. Prior notes instructing creation of a new child PlayerController are superseded by this confirmed project detail. `LookAtTrace` collision setup and component defaults were previously instructed and user replied yes, though no independent in-editor inspection occurred.

### Nice Interaction V2 — GameState setup confirmed, first interactable test planned (2026-10-20)

**User-confirmed:** existing Third Person PlayerController (already derived from `GSCModularPlayerController`) was given `AC_PC_Interaction`. User created a GameState Blueprint with `AC_GS_Interaction` and **assigned that GameState class in the existing GameMode**. The prior `LookAtTrace` project trace channel (Ignore default) and `AC_PC_Interaction` default trace variables were instructed, and user acknowledged completion. We have **not yet** confirmed the controller/component performs traces or the input mapping supports interaction during PIE.

**Next isolated test proposed (await user):** in `L_EnvironmentTest` find vendor example `BP_InteractableCube` in Content Browser, place one beside the Third Person spawn, Save, PIE, approach/look at it and report whether focus/prompt is displayed. This exact example actor is named by vendor documentation: https://niceshadow.gitbook.io/nice-interaction-system/replicated-multiplayer-interaction . If not found, find vendor-provided interactable and inspect `BP_MakeInteractable`, rather than inventing custom interface or input events yet. After focus test, check interaction input and verify a real interaction, then package and backup. **Do not claim this stage validated yet**.

### Nice Interaction V2 — initial interaction PIE verified, gamepad prompt investigation (2026-10-20)

**User confirms in PIE:** placed vendor example `BP_InteractableCube` **and** `BP_InteractableCampfire` in the scene; **both interact successfully**, and an on-screen `E` interact prompt appears. Therefore the PlayerController + GameState + LookAtTrace integration has passed a first **keyboard interaction smoke test** (not packaged yet).

**Current inquiry:** can Nice Interaction System V2 display gamepad button icons instead of an `E` prompt? Official Fab listing of **Nice Interaction System V2** (https://www.fab.com/listings/63b61e4a-dc11-4ee0-a6b1-f4860bd29198?lang=en) confirms focus/gamepad support, but the [vendor Nice Interaction documentation](https://niceshadow.gitbook.io/nice-interaction-system/integration-in-your-project) does **not** establish an automatic keyboard/gamepad input-icon switching widget. Do NOT confuse with **Nice Settings Menu V2** (a separate product that explicitly supports device-aware key widgets, https://www.fab.com/listings/4ef6bbf3-560e-419f-8693-70cad4a43fd5). **Next minimum experiment proposed, pending user:** find the existing binding/event responsible for `E` interaction, add `Gamepad Face Button Bottom` (Xbox A / PlayStation ✕) through whatever input system the vendor actually uses, test interaction and observe whether UI automatically changes. If interaction works but `E` remains, inspect the vendor's interaction prompt UMG widget/binding before considering a lightweight dynamic glyph solution. Do not promise automatic icon switching until verified with installed assets.

### Nice Interaction V2 — gamepad action confirmed; glyph UI unresolved (2026-10-20)

**User-confirmed:** PlayStation controller **Square (□)** executes the **same interaction as keyboard E**, on the working examples, without additional input mapping. Its correct Unreal Enhanced Input key is **`Gamepad Face Button Left`** (not `Gamepad Face Button Bottom`, which was incorrectly proposed in an earlier response). Thus native interaction works on both devices; **gamepad support is verified in PIE**, while **device-specific interaction prompt/glyph switching has NOT been confirmed**. The user suspects only the UI currently retains the "E" indicator.

**Next smallest inspection:** inspect the existing Nice Interaction UI before changing any input or replacing vendor systems. Open `AC_PC_Interaction` Blueprint and trace the widget class reference (e.g., Create Widget / widget class variable / UI components), or inspect `BP_MakeInteractable` widget components on an example if nothing is created in `AC_PC_Interaction`. Identify the exact widget/field that renders `E`; check for an existing device-sensitive key/icon binding first. Only then decide between exposing native variable/icon swap on last used input versus plugin extension. The Nice Interaction System V2 official feature page supports gamepad but does **not guarantee automatic gamepad icon changes**: https://www.fab.com/listings/63b61e4a-dc11-4ee0-a6b1-f4860bd29198?lang=en . **Nice Settings Menu V2** is a separate NiceShadow product with documented automatic key glyph UI; do NOT assume that functionality exists in Nice Interaction System V2 or purchase a new plugin just for a basic prompt without evidence.

### Nice Interaction V2 — mandatory integration complete; packaging next (2026-10-20)

**User-confirmed setup and PIE function:** `LookAtTrace` configured (default Ignore) and referenced from `AC_PC_Interaction`; the component was attached to the **existing Third Person PlayerController Blueprint deriving from `GSCModularPlayerController`**. `AC_GS_Interaction` added to a new GameState Blueprint and that GameState assigned to the actual GameMode. Vendor sample interactive **cube** and **campfire** placed in `L_EnvironmentTest`, and the user confirmed **both interact normally** with keyboard E and PlayStation gamepad Square (`Gamepad Face Button Left`), including E on-screen prompt. **Official vendor [integration quick guide](https://niceshadow.gitbook.io/nice-interaction-system/integration-in-your-project)** explicitly says these collision + components are the entirety of required setup; do not invent additional mandatory plugin configuration. Vendor [optional outline feature](https://niceshadow.gitbook.io/nice-interaction-system/add-outline-to-your-project-and-enable-it-in-your-map) and multiplayer replication workflows are not part of the minimal integration gate. Device-specific dynamic button glyph is still a **deferred UI enhancement** and need not block closure.

**Next required project quality gate (pending):** Package Windows Development using the configured test level with cube + campfire and run the .exe. Confirm that both interactions work via E and gamepad Square, and **GAS Companion Jump/Sprint, Stamina drain/regen, and HUD** are unaffected. If all pass, create a full-project **`06_NiceInteraction_Validated`** restore point (recommended next chronological number after existing `04` Jump and `05` JumpSprint backups), then progress to evaluating Black Eye and Ultimate Gameplay Camera. Don't call Nice Interaction `packaged-tested` or claim the `06` backup exists until user reports completion. Optional cosmetic glyph/outline work should not slow down integration research.

### Nice Interaction V2 — packaging and backup 06 confirmed (2026-10-20)

**User-confirmed:** Windows Development packaging completed successfully, with Nice Interaction V2 functioning correctly in the packaged build. User subsequently confirmed creating the full-project backup **`06_NiceInteraction_Validated`** (number/label as planned; the local folder path was not independently inspected). This supersedes older sections that marked packaging and checkpoint 06 as pending. Nice Interaction's current minimal integration stage is **closed**. Deferred optional input-glyph switching remains out of scope.

**Next research/integration:** adopt **Ultimate Gameplay Camera (UGC)** as the first gameplay-camera prototype, preserving the existing Third Person Character, GAS Companion PlayerController, Nice Interaction and Enhanced Input. Compare three *Camera Data Asset* presets (close/medium/far) through existing `BertaUltimateGameplayCameraExt` cycle tooling only after the primary UGC manager is confirmed working. No camera runtime/packaging validation is claimed yet; Black Eye cinematic integration follows separately.

### Ultimate Gameplay Camera — three-preset PIE switching verified (2026-10-20)

**User-reported progress:** UGC and `BertaUltimateGameplayCameraExt` are enabled in the UE5.8 Sandbox. The existing Third Person PlayerController (still derived from `GSCModularPlayerController`, with Nice Interaction attached) was configured initially with the vendor `BP_UGCCameraManager`. `Berta.UGC.Dump` confirmed an active UGC manager, and movement/Look worked in PIE. The user then added `BertaUGCCameraCycleComponent`, created `DA_Camera_Medium` plus Near/Far variants and attempted switching; editing the Data Asset alone caused no visible change, and `Berta.UGC.Next` reported **`success=false, storedCycleIndex=-1`**. The active preset was a vendor asset; merely including that asset in the cycle **did not fix the failure**.

**Critical integration lesson:** the user pointed out that a prior project had used a child Blueprint of `BP_UGCCameraManager` with its own **`Default Camera Data`**. The missing step was then prescribed for this project: child manager `BP_SandboxUGCCameraManager` with `Default Camera Data = DA_Camera_Medium`, assigned as `Player Camera Manager Class` of the **existing** controller. Shortly afterwards, the user **confirmed two successful `Berta.UGC.Next` camera changes in PIE**. We did not inspect the final Blueprint graph/asset assignments or capture a post-fix Dump, so this sequence establishes functional success but does **not** isolate the exact original `false` branch. Do not repeat the previous overconfident claim that a vendor preset outside the cycle alone caused everything.

**Camera preset experiment:** Medium screenshot values include Arm Length 100–250 and FOV 90–125; proposed Near 80–120 and FOV 80–90; proposed Far 350–450 and FOV 95–105; common 0.5-second arm/FOV blend. Three **data-driven styles of one gameplay camera**, not three CameraComponents. Names/values of final saved assets have not been independently inspected.

**Detailed reproducible runbook, exact error chronology, source-backed cycle conditions, presets and remaining tests:** [Ultimate Gameplay Camera — Sandbox integration](../research/plugins/ultimate-gameplay-camera-sandbox-integration.md). **All visible `UGC_CameraDataAssetBase` Details fields** from 10 screenshots: [Camera Data Asset reference](../research/plugins/ultimate-gameplay-camera-data-asset-reference.md). The original [Black Eye vs. UGC research](../research/plugins/camera-systems-comparison.md) remains research evidence, not a camera-package certification.

**Still pending:** retest `Berta.UGC.Previous`, post-switch regressions of GAS and Nice Interaction, UGC current Dynamic Spring Arm/collision configuration, Focus/dithering if required, Windows Development **with UGC presets**, and next full-project camera checkpoint (07 **not created or confirmed**). Black Eye cinematic integration is separate and not yet started.

### Ultimate Gameplay Camera — Windows packaging confirmed; backup 07 next (2026-10-20)

**User-confirmed:** after successful PIE switching with `Berta.UGC.Next`, the user packaged the Sandbox and reported **the packaged application works correctly**. This advances the UGC integration beyond the earlier PIE-only stage. The precise build configuration and a per-feature regression checklist were not independently logged; avoid presenting all optional UGC modifiers as tested.

**Next full-project restore point recommended:** `07_UGC_CameraPresets_Validated`, saved alongside the existing **`06_NiceInteraction_Validated`** as a separate backup, without overwriting 06. **Backup 07 is not yet user-confirmed as created.** No absolute local backup directory has been observed.

**Next research:** compare UGC's native Dynamic Spring Arm/predictive collision, Focus, Yaw/Pitch Follow, Camera Animation and data-driven state camera with what `BertaUltimateGameplayCameraExt` already provides (cycling, Dump, audit, preset diff). Focus on a small Sandbox experiment and only promote a reusable extension where repeated value exists. Proposed candidates, not committed implementation: SpringArm collision smoke test; GAS Sprint-driven camera preset with safe restoration; Focus on a target; diagnostic/audit warning when default camera asset is outside `CameraPresets`. Distinguish commercial UGC base features from separately sold add-ons.

### Ultimate Gameplay Camera — restore point 07 confirmed (2026-10-20)

**User-confirmed:** after UGC preset-cycle integration and successful Windows packaging, the user **created the complete-project backup 07**. The checkpoint was recommended as `07_UGC_CameraPresets_Validated`; **the exact local folder spelling and path were not independently inspected**. This supersedes the previous note marking checkpoint 07 as pending. Backup 06 remains the separate Nice Interaction baseline.

**Next BertaDevKit work to delegate:** improve the **read-only UGC Editor Audit** to check that the manager's configured **Default Camera Data** belongs to the existing `UBertaUGCCameraCycleComponent::CameraPresets` by actual asset identity, including correctly reading a Blueprint-derived manager's default configuration. Audit already checks global GameMode, Controller manager class, cycle presets, and selected preset properties, but not this contract. The user intends to pass the research/problem to the separate BertaDevKit ChatGPT project, which will prepare an implementation prompt for Codex. This is **a proposed improvement, not implemented**. Separately improve runtime `success=false` diagnostics later.

### Ultimate Gameplay Camera — UGC Dynamic Spring Arm and corner avoidance verified in PIE (2026-10-20)

**User-confirmed follow-up performed after backup 07:** the existing `BP_ThirdPersonCharacter` initially had native Unreal `CameraBoom` (`SpringArmComponent`) with Arm Length 400, offsets zero, `UsePawnControlRotation`, `InheritPitch/Yaw/Roll`, `DoCollisionTest` enabled, Probe Size 12, Probe Channel Camera, and Camera Lag off. `FollowCamera` used **Parent Socket = `SpringEndpoint`**. These values were observed in a user screenshot.

A **`BP_UGCSpringArmComponent`** from UGC was added as a sibling under `CapsuleComponent`, given the same baseline arm/rotation/collision properties and left with its UGC-specific collision defaults. `FollowCamera` was reparented from native `CameraBoom` to the UGC Spring Arm, retaining **`Parent Socket = SpringEndpoint`** and zero relative Location/Rotation.

**Important pitfall:** with **both** Spring Arms temporarily present, the camera appeared too centered on the player and the Near/Medium/Far distance changes were barely visible. The user **deleted the old `CameraBoom`** and confirmed that the camera/presets **worked correctly again**. This strongly suggests conflicting Spring Arm identification/settings, but the precise internal cause was **not traced with a before/after `Berta.UGC.Dump`**. For this Sandbox setup, leave **only one UGC Spring Arm**; do not leave the old native boom in place. Do not call a specific UGC component-resolution path proven.

**Functional PIE evidence:** the camera now **interpolates/smoothly adjusts** when near a wall and the user noticed it **anticipating the corner** during slow rotation. This supports successful visual predictive avoidance, but not a measured feeler trace or performance test.

**Validation boundary:** earlier **Windows packaging and backup 07 were completed before this Spring Arm replacement**. **A new packaged build after this change has not yet been confirmed**; no post-change checkpoint is confirmed. No change to BertaDevKit C++ runtime code was made during this UI migration. Full procedure, screenshots provenance, hierarchy and socket details: [UGC integration report — Dynamic Spring Arm](../research/plugins/ultimate-gameplay-camera-sandbox-integration.md#update--dynamic-spring-arm-migration-and-predictive-collision-2026-10-20).


### Ultimate Gameplay Camera — Camera Focus packaged, native Follow toggle identified (2026-10-24)

**Confirmación del usuario:** probó **Camera Focus en el ejecutable empaquetado** y reportó «el focus funciona». Este avance supera el histórico **Focus pendiente**. No se adjuntó log ni checklist de build/configuración; no marca como pasada la transición completa Exploración ↔ Focus, su restauración, la selección de objetivo dinámico ni otras regresiones.

**Hallazgos del prototipado (Blueprints proveedor UGC 1.92, UE 5.8):**

- El error de Focus «Trying to focus the camera on a target while yaw/pitch follow modifiers are enabled. This is not allowed.» depende de `IsYawMovementFollowEnabled OR IsPitchMovementFollowEnabled`.
- Yaw considera `Enable Yaw Movement Follow`. Pitch considera **tres flags OR**: `Enable Pitch Movement Follow`, `Trigger when Falling`, `Trigger on Slopes`. Desactivar solamente Pitch Enabled, o llamar `Disable Modifier` / `Remove Camera Modifier`, **no resolvió** la advertencia.
- `BP_UGCCameraManager` **ya trae la función Blueprint heredable `ToggleYawAndPitchFollow`** con las cuatro entradas correspondientes. Su graph modifica las `FollowSettings` de las instancias internas, no solo el Data Asset. Para suspensión requiere cuatro `false`; para restauración habría que guardar los cuatro valores originales. La función **no guarda ni restaura** por sí sola aunque su nombre diga Toggle.
- `BP_FocusTargetMethod_Test1` ejemplifica `GetAllActorsWithTag("FocusTest1")`, primer actor y localización para Focus (en la captura, X/Y del actor y Z=0); no prueba un sistema de selección de enemigos.
- **Yaw Follow automático no validado:** movimiento lateral/diagonal no produjo giro perceptible; debug no dio salida visible; variar Time Threshold no resolvió. No confundir con el cambio exitoso de Focus.
- Un informe de viabilidad Codex sobre UGC C++ indicó que los FollowSettings efectivos de los modifiers Blueprint **no tienen acceso C++ tipado público** en UGC 1.92. **No hubo** extensión/runtime bridge nuevo, commit, build ni tests de esa feature en BertaDevKit. Para Sandbox priorizar la función heredada del proveedor.

**Estado de arquitectura:** persisten el único `BP_SandboxUGCCameraManager`, el ciclo de presets Medium/Near/Far y el UGC Spring Arm. Las funciones custom `SuspendMovementFollow`, `RestoreMovementFollow`, `SetFocusActive` y `SetCameraMode` son **propuestas pendientes**, no un sistema reversible validado. La investigación recomienda guardar la configuración de Follow original solo cuando exista necesidad de restaurarla; no sobreescribir el snapshot ante suspensiones repetidas. No existe nuevo backup confirmado después del backup 07, anterior al Spring Arm.

**Próximo gate:** confirmar en PIE y packaged la transición Focus ON/OFF y restauración de los cuatro flags; después probar presets durante Focus, Look, collision, GAS/Nice Interaction y entradas de mouse/gamepad. Ver [informe cronológico y capturas descritas](../research/plugins/ultimate-gameplay-camera-sandbox-integration.md#update--camera-focus-y-yawpitch-follow-packaged-smoke-test-2026-10-24).

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


### Ultimate Gameplay Camera — official PDF reviewed; documented Focus helpers (2026-10-26)

El usuario incorporó a las fuentes del proyecto ChatGPT **`Ultimate Gameplay Camera Documentation.pdf` (65 páginas)**. Se revisó el documento completo (texto y páginas de diagramas). Matriz de capítulos, funciones Blueprint, estado de experimentos y pruebas siguientes: [UGC feature matrix](../research/plugins/ultimate-gameplay-camera-feature-matrix.md).

El manual muestra **`Toggle Camera Focus`**, **`Set Camera Focus Settings`**, **`Set Camera Focus Targeting Method`** en el manager (PDF p. 20); para elegir target usa Blueprint hijo de **`UGC_IFocusTargetMethod`** y override **`GetTargetLocation`**, **llamando al Parent obligatoriamente** (pp. 21–23). El target method retorna posición y opcionalmente referencia al actor, usada para Line of Sight. Esto reemplaza la idea de replicar el sistema de Focus; **la selección de un actor explícito durante gameplay continúa pendiente**. El nodo general real de debug se llama **`Toggle All UGCModifiers Debug`** (FAQ p. 57), no el nombre inferido previamente.

El manual ensamblado **no incluye System Setup ni la guía de Advanced/Dynamic Spring Arm**; esos temas siguen con fuentes aparte. Persisten las confirmaciones anteriores de Focus empaquetado y colisión predictiva observada en PIE; no se declara nuevas verificaciones de gameplay, packaging o backup por leer el manual.

### UGC + GAS Target Lock + Berta Actor Tags — funcional, empaquetado pendiente (2026-10-26)

**Confirmación del usuario:** `GA_CameraFocusToggle` está integrada como Ability **instantánea por pulsación**, no persistente. Con input Middle Mouse/R3, la nueva selección de target sigue esta política: no hay target → A: Focus ON; target A y nuevo candidato vacío: OFF; A y A: OFF; A y B: cambiar foco B. El usuario reporta que **todo funciona**, incluidos los casos de retorno a cámara libre/restauración Yaw/Pitch Follow, desaparición/destrucción del target y transición entre targets discutidos en la revisión.

La selección se alimenta de `Sphere Overlap Actors` con criterio de alineación al eje visual (`Dot Product`) y elegibilidad por **Actor Tag** `Berta.ActorTag.Targeting.Focusable`, proveniente de la nueva colección de **40 etiquetas / 16 subfamilias** añadida al plugin base BertaDevKit. Los Actor Tags son `FName`, no un ASC ni Gameplay Tag Container; el uso de un `Gameplay Tag` configurado más `Get Tag Name → Actor Has Tag` centraliza el nombre. `BP_SandboxFocusTargetMethod` ya observa `In Owner = BP_ThirdPersonCharacter`, por lo que puede consultar el actor seleccionado del personaje; UGC ejecuta el Focus. **No se inspeccionó el Blueprint final ni se verificaron todas sus propiedades exactas**.

**Estado:** smoke test de funcionamiento comunicado por usuario. **Pendiente:** empaquetar y probar el **Target Lock completo** en Windows Development y crear un nuevo respaldo del proyecto, que hasta ahora no se confirmó. La antigua prueba packaged de Focus básico (2026-10-24) no sustituye esa regresión. Notas técnicas completas: [UGC Sandbox integration](../research/plugins/ultimate-gameplay-camera-sandbox-integration.md#update--camera-focus-target-lock-funcional-con-gas-companion-y-berta-actortag-2026-10-26).

### UGC Target Lock — prueba packaged confirmada (2026-10-26)

**User-confirmed:** al finalizar la integración de `GA_CameraFocusToggle` y solicitar una prueba del flujo completo en el ejecutable empaquetado, el usuario informó «funciona». **Estado actual: smoke test de Target Lock dinámico exitoso también en build empaquetada**. Este resultado actualiza los apartados anteriores que decían «empaquetado pendiente» y es distinto de la prueba anterior de Focus básico del 24-10. No hay logs ni cobertura pormenorizada por caso del paquete, por lo que evitar afirmar verificaciones no reportadas de red/performance/los casos individuales en el ejecutable.

**Único cierre administrativo del hito pendiente:** crear y confirmar un **nuevo backup del Sandbox**; último checkpoint anteriormente confirmado: 07, antes de la migración de Spring Arm. [Registro completo del hito](../research/plugins/ultimate-gameplay-camera-sandbox-integration.md#update--target-lock-empaquetado-confirmado-2026-10-26).

### Combo Graph + GAS Companion — Light/Heavy y stamina por nodo validados (2026-10-28)

**Estado: hito funcional cerrado por confirmación explícita del usuario** («Ya funcionó todo correctamente. Así podemos dar por cerrado esto»). La prueba se desarrolló sobre el mismo personaje del Sandbox con **GAS Companion**, **Enhanced Input**, **Combo Graph** y el atributo **`GSCAttributeSet.Stamina`** que ya se usaba para Jump/Sprint.

**Preparación de animaciones:** se descargaron dos animaciones de **Mixamo** y se retargetearon satisfactoriamente a **UE5 Manny mediante el retargeting nativo de Unreal 5.8**; no fue necesario comprar Retarget Guru ni Mixamo Animation Retargeting 2. La animación Light contiene cuatro puñetazos alternados izquierda/derecha; la Heavy, un cross izquierdo y un uppercut. Se optó por Animation Montages con secciones para reproducir un golpe por pulsación, en vez de ejecutar automáticamente la secuencia de varios impactos.

**Integración confirmada por el usuario:**
- **Light Attack:** secuencia de cuatro golpes independientes, avanzando por input y Combo Windows de Combo Graph.
- **Heavy Attack:** secuencia de dos golpes independientes (cross/uppercut).
- Dos **Input Actions**, dos **Gameplay Abilities** integradas con GAS Companion y sus respectivos grafos de Combo Graph; los nombres de assets y mappings definitivos no fueron inspeccionados aquí, por lo que se conservan como referencias conceptuales y no como manifest exacto.
- **Cost Gameplay Effects por nodo:** Gameplay Effects instantáneos que reducen `GSCAttributeSet.Stamina`, configurados en el **Cost Gameplay Effect de cada nodo Combo Graph**, no como cobro único en `GA_LightAttack`/`GA_HeavyAttack`. El usuario confirmó que funcionan los costos y la lógica de no ejecutar el siguiente golpe cuando la stamina es insuficiente. Como referencia de la guía se propusieron `GE_LightAttack_StaminaCost` (−5) y `GE_HeavyAttack_StaminaCost` (−15); comprobar nombres/valores reales en el proyecto antes de documentarlos como definitivos.
- **Commit / doble cobro:** evitar duplicar un Cost Gameplay Effect en la Gameplay Ability si Combo Graph ya aplica el costo por cada nodo. `Commit Ability` solo verifica/aplica los costos de la propia Ability y sus restricciones configuradas, no sustituye el costo interno de los nodos.

**Detalle observado y aceptado para este prototipo:** el Montage de Light requirió `Blend In = 0` y `Blend Out = 0` para encadenar golpes muy próximos; la vuelta al estado de locomoción al salir del último golpe es visualmente abrupta. El usuario aceptó esa limitación para la validación R&D; no se añadió un framework de animación ni una recuperación especial.

**Decisión deliberada de alcance:** posponer **Combo Collision**, sockets y hit traces, daño y Gameplay Effects sobre enemigos, Gameplay Cues, branching Light→Heavy, ataques cargados e IA hasta que aparezca un caso de uso. El sistema de combos y costo por nodo ya demostró integración suficiente con GAS Companion para cerrar esta fase. No crear todavía una extensión nueva de BertaDevKit: no hay evidencia de glue reutilizable repetido.

**Evidencia y límites:** prueba funcional confirmada por el usuario; no se inspeccionaron los Blueprint graphs finales ni se aportó confirmación específica de una **build empaquetada posterior a Combo Graph**. Tampoco se confirmó un nuevo backup posterior al último checkpoint anterior. Estos controles quedan como tareas operativas pendientes si se desea congelar una baseline de distribución; **no bloquean el cierre del experimento funcional**.

**Siguiente candidato sugerido:** Dragon IK, para un smoke test de adaptación procedural de pies en terreno irregular, comparando con soluciones nativas y preservando la integración actual.

### Narrative Tales + Nice Interaction V2 — inicio de diálogo en PIE validado (2026-10-29)

**User-confirmed:** el usuario probó en PIE que una interacción de **Nice Interaction System V2** inicia y **muestra correctamente un diálogo de Narrative Tales** («ya lo testee haciendo que al interactuar aparezca el dialogo, funciona»). Esta integración reutiliza la infraestructura del Sandbox: PlayerController existente con GAS Companion / Nice Interaction, componente Narrative, Common UI + Narrative Common UI, y un diálogo de prueba configurado con el speaker **`TestNPC`**. Durante la guía se propuso un interactable de prueba y el flujo de evento de interacción → `Begin Dialogue` sobre el Narrative Component; los nombres definitivos de widgets/componentes y el grafo final de Blueprint **no fueron inspeccionados**.

**Detalle de editor a conservar:** al intentar seleccionar el speaker desde un campo `None` en el editor del diálogo se produjo una recursión infinita reportada por el usuario; se logró crear un nodo usando el menú contextual **`Add Dialog Line for TestNPC`**. El hecho de que el diálogo aparezca en PIE no demuestra que ese selector/editor bug esté corregido. No modificar el plugin comercial sin necesidad ni atribuir el fallo a una causa no verificada.

**Alcance demostrado:** Nice Interaction → evento de interacción → Narrative Dialogue visible en pantalla. **Todavía no se confirmaron** selección y navegación por las dos respuestas con teclado/gamepad, fin limpio de diálogo y retorno del input al juego, elección de ramificaciones/quest logic, ni packaged build o nuevo backup para esta integración. Tampoco se probó interoperabilidad con Menu System Pro V2 de Moonville; revisar la futura colisión de `Common Input Settings → Input Data` y perfiles de controller cuando el producto se integre, sin reemplazar ciegamente la configuración Narrative actual.

**Próximo smoke test recomendado:** seleccionar cada respuesta en `D_TestDialogue` mediante mouse y gamepad; verificar cierre del menú y retorno del control a juego; luego pasar a una quest mínima de Narrative Tales, sin introducir UI custom ni más sistemas.
