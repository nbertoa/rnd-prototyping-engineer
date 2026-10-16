# Sandbox Prototype

Unreal Engine 5.8 integration sandbox used to install, configure, and validate reusable plugins before adopting them in a concrete product or prototype.

The goal is to reduce integration risk, understand dependencies and interoperability, and preserve a known-good reference setup without coupling plugin experimentation to the Biblical Sanctuary project or another product.

---

## Current baseline

- Unreal Engine 5.8.
- First-person playable baseline is working.
- Character, GameMode, and PlayerController may remain Blueprint-based while the goal is plugin integration and validation. Move logic to C++ only when there is a concrete architectural or reuse benefit.
- BertaDevKit is integrated.
- Third-party plugins are being integrated and validated incrementally.

---

## Moonville integration

Configure **Menu System Pro first**, then integrate the other Moonville systems against the same project-level UI/input configuration.

### Required Unreal plugins

For the current Moonville setup, enable:

- HTTP
- JSON
- WebSocket Networking
- JSON Blueprint Utilities
- Common UI

`Enhanced Input` is also required by the stack, but in Unreal Engine 5.8 it is available/enabled by default in the current project baseline.

Keep these dependencies documented as part of the Moonville reference configuration so a fresh project can reproduce the setup without rediscovering them.

### Menu System Pro — intended simplification

Graphics settings should expose only:

- Display Mode
- Resolution
- Quality Preset (Low / Medium / High / Epic)

Remove the individual graphics controls from the player-facing settings UI.

Localization/UI decisions for the current prototype baseline:

- English only for now; hide the language selector while there is only one supported language.
- Remove Moonville branding/node from the Start Screen.
- Keep only Unreal branding on the Start Screen.
- Remove the game-name text from the Menu System UI; final branding should come from the background/key art.
- Use a black background temporarily until final art exists.

---

## Validation rule

Do not treat a plugin as fully integrated merely because it compiles or opens in the editor. For each system, verify its minimum runtime workflow in PIE and record any project settings, input mappings, components, interfaces, or cross-plugin assumptions required to reproduce the setup.
