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
