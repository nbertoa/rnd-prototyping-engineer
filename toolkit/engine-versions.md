# Engine versions — which Unreal version to use for what

Last reviewed: 2026-11-19.

## Decision: two tracks

| Track | Engine | Used for | Plugins |
|-------|--------|----------|---------|
| **Stable stack** | UE 5.8 (`Sandbox Prototype`) | Briefs that depend on the commercial plugin stack (cameras, GAS Companion, narrative, interaction, UDS, Chameleon…); packaged-build validation | Full owned stack, already validated with packaged builds |
| **AI track** | UE 5.9 Preview (separate, lean project) | Briefs about agents in the editor: Unreal MCP, Unreal Assistant, Unreal Canvas, expanded Python (briefs 001 and 003) | Engine-native features + BertaDevKit (recompiled) + free content. Commercial plugins only one at a time, in a copy, when a brief needs them |

**Do not move the main Sandbox to 5.9 Preview yet.**

## Why

- **5.9 is a preview.** Epic built the 5.9 MegaJam Preview in about a week with a shorter QA pass than usual, is still cleaning up agent-written code, and plans to refactor parts of the web renderer before the full release. The full 5.9 release is expected next year (2027).
- **The owned plugin stack does not declare 5.9 yet.** On 2026-11-19, Fab's engine-version filter already offers 5.9, and 23 *Tools & Plugins* listings declare it, but none of the owned stack appeared among those checked. Of the wishlist, only **MeshBlend** and **MeshPack** declare 5.9. **Logic Driver Pro** lists 4.19–4.27 and 5.0–5.8 and states it supports the latest *official* Unreal release.
- **The AI tooling lives in 5.9.** Unreal MCP's expanded toolsets, Unreal Assistant and Unreal Canvas are the subject of briefs 001 and 003, so those briefs need 5.9. They do not need the commercial stack: native features and BertaDevKit are enough, and the results are easier for others to reproduce.

## Porting a plugin to 5.9 Preview (only when a brief needs it)

1. Work in a copy of the 5.9 project, never the 5.8 Sandbox.
2. Code plugins that ship source can often be recompiled for a newer engine, but this is unsupported until the vendor lists the version. Content / Blueprint-only assets usually open in newer versions; still test them.
3. Record the result per plugin (compiles / runs in PIE / packages) in `plugin-inventory.md`.
4. If it needs source changes, stop: it is not worth maintaining a private fork of a commercial plugin for a preview engine.

## When to migrate the stable stack

Migrate the Sandbox when **both** are true:

- UE 5.9 has its full (non-preview) release, and
- the plugins used by active briefs declare 5.9 on Fab (or compile and package cleanly in a test copy).

Until then, check this file again at the start of each build week.

## Note on "last version before UE6"

Epic described 5.8 as the last *planned* major UE5 release, then announced 5.9 (preview now, full release in 2027). UE6 early access is targeted around the end of 2027. 5.9 is likely the last UE5 release, but Epic has not stated that formally, so plans should not depend on it.
