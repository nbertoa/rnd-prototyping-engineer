# Sandbox Prototype

Unreal Engine 5.8 integration sandbox used to install, configure, and validate reusable plugins before adopting them in a concrete product or prototype.

The goal is to reduce integration risk, understand dependencies and interoperability, and preserve a known-good reference setup without coupling plugin experimentation to the Biblical Sanctuary project or another product.

---

## Current baseline

- Unreal Engine 5.8.
- First-person playable baseline is working.
- Character, GameMode, and PlayerController remain Blueprint-based while the goal is plugin integration and validation. Move logic to C++ only when there is a concrete architectural or reuse benefit.
- BertaDevKit is integrated.
- Third-party plugins are being integrated and validated incrementally.

---

## Moonville stack

Detailed reproducible setup, decisions, validation results, troubleshooting notes, and current integration progress are maintained in:

**[Moonville Stack Integration — UE 5.8 Sandbox](../research/plugins/moonville-stack-integration.md)**

Current status:

- **Menu System Pro V2 Common UI** — integrated and fully validated, including Windows Development packaged-build testing.
- **Inventory System Pro** — present; combined Menu + Inventory + Inspection integration in progress.
- **Inspection System Pro** — present; combined Menu + Inventory + Inspection integration in progress.
- **Map System Pro** — present; dedicated integration not started yet.

The current strategy is to use Menu System Pro as the shared Common UI / Enhanced Input foundation and integrate Inventory and Inspection against that known-good baseline before moving to Map System Pro.

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
