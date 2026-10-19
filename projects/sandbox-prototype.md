# Sandbox Prototype

Unreal Engine 5.8 integration sandbox used to install, configure, and validate reusable plugins before adopting them in a concrete product or prototype.

The goal is to reduce integration risk, understand dependencies and interoperability, and preserve a known-good reference setup without coupling plugin experimentation to the Biblical Sanctuary project or another product.

---

## Rebuild 2026-10-19 — plugin inventory and verification

A fresh **UE 5.8 Blank C++** Sandbox was created after the GAS Companion Third Person template failed to create successfully. Plan: add the stock Unreal **Third Person** Feature Pack (not GAS Companion's template), then integrate and test plugins incrementally with restore points. **Moonville is postponed until the other selected plugins are validated.** The new Sandbox's installed/working plugin list has **not** been verified yet.

### User-reported plugins available to consider

The user named the following plugins on 2026-10-19. **This is an inventory of plugins they report having, not proof they are installed, enabled, or working in the new Sandbox**. Keep the reported names rather than silently equating similar products.

1. Animation Tools Bundle
2. Dragon IK
3. Runtime Audio Importer
4. Black Eye
5. Ultimate Gameplay Camera
6. Auto Size Comments
7. Blueprint Assist
8. Electronic Notes *(user's wording; verify whether this is **Electronic Nodes**)*
9. Global Search Pro
10. Graph Printer
11. Combo Graph
12. GAS Companion
13. Global Variable System
14. Low Entry Extended Standard Library
15. Narrative Tales
16. State Tree Tools
17. PCG Extension *(user's wording; verify exact plugin identity before treating it as PCGEx)*
18. Screen Space Fog Scattering
19. HTTP JSON and WebSocket Blueprint Plugin *(user identifies this as the plugin previously called 'UForge' in discussion; verify exact asset name in the editor)*

Also previously confirmed in the former Sandbox: **BertaDevKit**, the user's own toolkit. **Nice Interaction System V2** was also discussed for this Sandbox and is separate from Moonville, but was not included in this particular 19-item inventory. The four Moonville systems remain on the future integration list: Menu System Pro V2, Inventory System Pro, Inspection System Pro and Map System Pro.

**Important naming distinction:** the vendor plugin name reported above, **HTTP JSON and WebSocket Blueprint Plugin**, must not automatically be conflated with Unreal's **HTTP, JSON and WebSocket Networking** prerequisite or with JSON Blueprint Utilities. Check each actual plugin descriptor before recording equivalence.

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
