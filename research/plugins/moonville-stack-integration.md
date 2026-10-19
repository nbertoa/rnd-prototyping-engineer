# Moonville Stack Integration — UE 5.8 Sandbox

## Purpose

> **Documentation rule (2026-10-19):** Before returning to this historical Moonville integration record, consult the [official documentation index for all 27 Sandbox plugins](plugin-documentation-index.md), especially the four vendor-authored Moonville Google Docs and the supplied PDF names. This document primarily records the **previous First Person Sandbox** integration. In the **new Blank C++ + Third Person Sandbox**, Moonville has **not yet been reinstalled**. Do not treat old Blueprint screenshots/settings as proven for the new assets.


This document is the reproducible integration record for the Moonville systems currently being validated in **Sandbox Prototype**.

It captures the project-level setup, integration sequence, configuration decisions, validation results, troubleshooting notes, and the exact point reached in the combined Menu + Inventory + Inspection integration.

It is intentionally **not a replacement for the vendor documentation**. When a new project is created or a plugin version changes, the official PDFs remain the source for version-specific details. This note preserves the known-good integration baseline so the work does not have to be rediscovered from scratch.

The current Sandbox baseline is **Unreal Engine 5.8**. The available Moonville documentation is written for earlier UE5 releases, including UE 5.7, so the Sandbox itself is the evidence that the configuration below works on the current 5.8 setup.

---

## Moonville systems in the Sandbox

| System | Present in project | Integration status |
| --- | --- | --- |
| Menu System Pro V2 Common UI | Yes | Integrated, runtime-tested, save/load-tested, and packaged-build-tested |
| Inventory System Pro | Yes | Combined integration in progress |
| Inspection System Pro | Yes | Combined integration in progress |
| Map System Pro | Yes | Added to the project; dedicated integration not started yet |

The intended strategy is to establish **Menu System Pro as the shared UI/input foundation**, then integrate Inventory and Inspection against that same Common UI / Enhanced Input configuration. Map System Pro is kept separate until the first three systems are stable.

---

# 1. Shared prerequisites

Before treating the Moonville stack as ready, the required Unreal plugins must be available.

## Required Unreal plugins

Enable:

- `HTTP, JSON and WebSocket Networking`
- `JSON Blueprint Utilities`
- `Common UI`
- `Enhanced Input`

Important: `HTTP, JSON and WebSocket Networking` is **one Unreal plugin**, not three separate plugins.

`Enhanced Input` is required by the stack. In the current Unreal Engine 5.8 Sandbox baseline it was already available/enabled by default.

For a fresh project, configure the Unreal dependencies before relying on Menu System Pro. The Menu System Pro documentation warns that adding the asset before its dependencies can leave assets in a broken state and may require removing/re-adding the asset.

---

# 2. Menu System Pro

## 2.1 Common Input project settings

In **Project Settings → Game → Common Input**:

- Enable `Enhanced Input Support`.
- Enable `Allow Out Of Focus Device Input`.
- Set **Input Data** to `BP_MenuUIInputData`.

For **Platform Input → Windows**:

- Default Gamepad Name: `XSX`.
- Controller Data:
  - `BP_MenuControllerData_KBM`
  - `BP_MenuControllerData_XSX`
  - `BP_MenuControllerData_PS5`
  - `BP_MenuControllerData_Switch`

These settings establish Menu System Pro as the shared Common UI input configuration for the project.

---

## 2.2 Enhanced Input project settings

In **Project Settings → Engine → Enhanced Input**:

- Enable `Enhanced Input User Settings`.

In **Project Settings → Engine → Input**:

- Default Player Input Class: `EnhancedPlayerInput`.
- Default Input Component Class: `EnhancedInputComponent`.

In **Project Settings → Engine → General Settings**:

- Game Viewport Client Class: `CommonGameViewportClient`.

---

## 2.3 Game Instance

The Sandbox does not currently need a custom Game Instance, so the project uses:

- `BP_MenuSystemGameInstance`

Set it in:

**Project Settings → Maps & Modes → Game Instance Class**.

`BP_MenuSystemGameInstance` provides the Menu System settings manager and save-game manager infrastructure.

Its `ConfigRedirector` was verified to point at the intended Global Menu Config.

If a future project needs its own Game Instance instead, use the vendor's `BPI_GameInstanceExtension` integration path rather than replacing this logic ad hoc.

---

## 2.4 Initial vendor smoke test

After the project settings above were configured:

1. Restart Unreal Engine.
2. Open the vendor example `SilenceMenuLevel`.
3. Verify the example menu flow before integrating custom project classes.

### Known non-blocking log issue

During the example test, `LogUIActionRouter` produced messages similar to:

`Cannot create action binding for widget ... no action provided`

The messages appeared in specific Extras menus and when the Decision Dialog opened on quit, but those menus and the dialog continued to function correctly.

No global input configuration was changed to suppress this because the behavior was functional. Treat this as a **known non-blocking log issue** unless it begins to affect input or UI behavior.

---

# 3. Menu System Pro integration into the Sandbox First Person baseline

The Sandbox keeps its own Blueprint-based gameplay classes rather than adopting Moonville's example classes directly.

Current project-owned baseline:

- custom Blueprint Character
- custom Blueprint GameMode
- custom Blueprint PlayerController

This is intentional. The Sandbox is an integration laboratory, so there is no reason to move these classes to C++ until there is a concrete architecture or reuse benefit.

## 3.1 Play level

In the Sandbox play level:

- Add `BP_MenuSystemActor`.

The project keeps its own GameMode, so instead of switching to Moonville's example play GameMode:

- add `BP_MenuSystemGameModeComponent` to the project's GameMode.

---

## 3.2 PlayerController

Add to the project's own PlayerController:

- `BP_MenuControllerComponent`

This preserves the project's PlayerController while incorporating Menu System Pro's controller/input behavior.

---

## 3.3 Character

The project keeps its own Character.

The required Menu System Pro Character-side logic was copied/merged from `BP_MenuSystemCharacter`, including the logic needed for:

- Menu System initialization.
- Opening the in-game menu.
- Axis inversion / sensitivity settings behavior.
- Enhanced Input integration.

When merging possession logic, preserve the real Character possession flow rather than creating duplicate event paths.

The Menu System FAQ specifically calls out `Set Input Mode Game Only` from the Character's possession path as important for movement/input behavior.

---

# 4. Menu levels and Level Meta Data

## 4.1 Global Menu Config

The active `DA_GlobalMenuConfig` was configured so Menu System Pro loads the Sandbox's intended levels rather than the vendor play level.

Current baseline:

- Main/Menu level: `SilenceMenuLevel` is still used as the known-good menu level for the Sandbox baseline.
- Play level: the Sandbox's own First Person play map.

The exact project-owned play-map asset name should be treated as project data; the important integration rule is that the Global Menu Config must reference the actual play level used by the Sandbox.

---

## 4.2 Level Meta Data

For every new level used by Menu System Pro:

1. Create/duplicate the appropriate Level Meta Data Data Asset.
2. Add it to the Level Meta Data map/list in `DA_GlobalMenuConfig`.
3. Use a map key that matches the **exact Unreal level name**.

Important vendor rule:

> The Level Meta Data map key must exactly match the level name, including accidental spaces.

This metadata is used by the Menu System loading-screen flow and other level-aware UI logic.

### Debugging rule

If Menu System Pro reports that Level Meta Data cannot be found even though the Data Asset appears correct:

- place a breakpoint in `WBP_MenuContainerBase → GetLevelMetaData`;
- inspect the incoming `LevelName`;
- compare it directly with the key in `DA_GlobalMenuConfig`.

During Sandbox setup, the system temporarily attempted to resolve `SilencePlayLevel` after the custom level had already been configured. The problem stopped occurring after further testing, but the exact root cause was not isolated, so do not encode an unverified explanation as fact.

---

# 5. Menu System Pro validation result

Menu System Pro is considered **fully validated for the Sandbox baseline**, not merely installed.

Verified in runtime:

- Main Menu opens correctly.
- New Game loads the Sandbox play level.
- In-Game Menu opens/closes correctly.
- Settings can be changed and persist.
- Save Game works.
- Load Game works.
- Return to Main Menu works.
- Level Meta Data resolves correctly.
- The system works with the Sandbox's own Character, GameMode, and PlayerController.

---

## 5.1 Packaged-build validation

A Windows **Development** package was produced and tested outside the Editor.

Packaging setup included:

- `English` in Localizations to Package.
- Internationalization Support: `All`.
- Menu level included in the packaged map list.
- Sandbox play level included in the packaged map list.

The packaged executable was tested successfully for:

- Main Menu.
- New Game.
- In-Game Menu.
- Settings changes.
- Save.
- Load.
- Return to Main Menu.
- Closing and reopening the executable with persisted settings/state.

This is the current strongest evidence that the Menu System baseline works correctly in UE 5.8.

---

## 5.2 Deferred Menu customization

A simplified player-facing Menu System configuration was discussed, including:

- Display Mode.
- Resolution.
- Quality Preset only (`Low / Medium / High / Epic`).
- Removing individual graphics controls.
- English-only UI / hidden language selector while English is the only supported language.
- Removing Moonville branding.
- Keeping Unreal branding.
- Removing game-name text from the menu UI.
- Temporary black background until final key art exists.

**None of these customizations are part of the current integration baseline.**

The decision for Sandbox Prototype is to keep the vendor base substantially intact and defer product-specific UI customization until a concrete project needs it.

---

# 6. Combined Inventory System Pro + Inspection System Pro integration

Moonville provides a dedicated **Menu + Inventory + Inspection System** integration guide. Because Menu System Pro was already validated first, Inventory and Inspection are being integrated together against the established Menu/Common UI configuration rather than installing each one independently from scratch.

The vendor guide explicitly recommends using the Menu System Pro UI Input Data and Controller Data as the shared project configuration when combining the systems.

---

## 6.1 Gameplay Tags

In:

**Project Settings → Gameplay Tags → Gameplay Tag Table List**

add:

- `DT_InventoryTags`

This Data Table supplies the example Gameplay Tags used by Inventory System Pro, including equipment-related tags.

Do not recreate these tags manually when reproducing this baseline; reference the supplied Data Table.

---

## 6.2 PlayerController components

The Sandbox continues to use its own PlayerController.

Add:

- `BP_InventoryControllerComponent`
- `BP_InspectionControllerComponent`

The existing Menu System component remains present:

- `BP_MenuControllerComponent`

Conceptually the project-owned PlayerController now hosts all three Moonville controller-side integration components.

---

## 6.3 Character components

Open:

- the Sandbox's own Character;
- `BP_InventoryCharacter_Horror`;
- `BP_InspectionCharacter`.

Copy/paste the vendor-configured components into the Sandbox Character rather than adding fresh empty components manually.

Current copied component set:

- `BP_InventoryCharacterComponent`
- `BP_CharacterEquipmentComponent`
- `BP_InventorySurvivalComponent`
- `BP_InteractionComponent`
- `BP_InspectionCharacterComponent`

The vendor page text says "four components", but the accompanying screenshot shows these **five** components. The Sandbox followed the screenshot and copied all five.

Copy/paste is important because it also copies the component settings. Manually adding clean components would require reproducing those settings and can produce integration errors if anything is missed.

---

## 6.4 `OnPossessed_Client` initialization

The Inventory and Inspection initialization calls were merged into the Sandbox Character's existing possession flow.

Procedure used:

1. From `BP_InventoryCharacter_Horror`, copy the four `OnPossessed_Client` initialization calls into the Sandbox Character.
2. Connect them into the existing `OnPossessed_Client` flow rather than creating an unrelated duplicate path.
3. From `BP_InspectionCharacter`, copy its `OnPossessed_Client` call and append it after the Inventory calls.
4. Ensure the PlayerController obtained by the existing cast is connected to the new initialization function calls where required.

This keeps Menu, Inventory, and Inspection initialization inside one coherent possession path.

---

## 6.5 Shared Character Input Mapping Context

Open:

- `DA_GlobalMenuConfig`
- `DA_GlobalInventoryConfig`
- `DA_GlobalInspectionConfig`

Set the Player/Character IMC used by all three systems to the same Input Mapping Context used by the Sandbox Character:

- `IMC_Default`

For Menu System Pro, the Character IMC is part of its `Initially Added Input Mapping Contexts` configuration.

Do not remove or overwrite the other system-specific IMCs from the vendor configs.

---

## 6.6 Shared Back action

The combined integration must use one Common UI Back Input Action across the systems.

In:

- `DA_GlobalInventoryConfig`
- `DA_GlobalInspectionConfig`

set:

- Back Input Action: `IA_UI_Back`

This must be the same `IA_UI_Back` used by the Menu System / Common Input Data, not a different Input Action that happens to have a similar name.

Then update the menu IMCs:

### `IMC_InspectionMenu`

Replace the Inspection-specific back action (`IA_UI_InspectionBack`) with:

- `IA_UI_Back`

### `IMC_InventoryMenu`

Replace its system-specific Back action with:

- `IA_UI_Back`

The goal is one consistent Back action across Common Input, Menu, Inventory, and Inspection.

---

## 6.7 Shared interaction action

In both:

- `DA_GlobalInventoryConfig`
- `DA_GlobalInspectionConfig`

set:

- Interaction Input Action: `IA_Interact`

This should be the same `IA_Interact` that the Sandbox Character uses in its Character IMC for world interactions.

---

# 7. Current stopping point

The integration is currently complete through the shared input configuration above.

The **next vendor step has not yet been applied**:

1. Open `WBP_IngameMenu`.
2. Remove the placeholder Discovery and Inventory widgets.
3. Insert:
   - `WBP_DiscoveryMenu`
   - `WBP_InventoryMenu_Horror`
4. Continue with the widget configuration and Event Graph integration described by the combined guide.

Do not mark Inventory System Pro or Inspection System Pro as fully integrated yet. They are currently **integration-in-progress**.

---

# 8. Reproduction / validation rules

For future projects using this baseline:

1. Add the Moonville assets to the project.
2. Enable/verify Unreal dependencies first.
3. Establish Menu System Pro and Common UI / Enhanced Input first.
4. Validate the Menu System independently before adding Inventory/Inspection integration complexity.
5. Keep project-owned Character, GameMode, and PlayerController classes when appropriate; integrate Moonville through components and required event/function logic.
6. Unify shared Input Actions rather than allowing each asset to keep a separate Back action.
7. Keep one project Character IMC (`IMC_Default` in the current Sandbox) referenced consistently by Menu, Inventory, and Inspection configs.
8. Smoke-test after each integration stage.
9. Do not consider the integration complete until it also works in a packaged build.

This sequence makes failures easier to isolate and preserves a known-good reference configuration.

---

# 9. Vendor documentation used

Current source documents available for this stack:

- `Menu System Pro V2 Common UI Documentation UE5.pdf`
- `Inventory System Pro Documentation.pdf`
- `Inspection System Pro Documentation [Common UI].pdf`
- `Map System Pro Documentation.pdf`

Most of the cross-system work above comes from the Menu System Pro chapter:

- **Integration Guide: Menu + Inventory + Inspection System**

The Inventory documentation was also used to confirm the role of `DT_InventoryTags`.

When the integration continues, update this document at each validated milestone instead of keeping the discoveries only in chat history.
