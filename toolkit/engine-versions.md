# Engine versions — which Unreal version to use for what

Last reviewed: 2026-11-19.

## Decision: UE 5.8 for everything, 5.9 as a later follow-up

| Track | Engine | Used for |
|-------|--------|----------|
| **Main** | UE 5.8 (`Sandbox Prototype`, full plugin stack) | All current briefs, including the AI-agent briefs (001, 003) through the official Unreal MCP plugin |
| **Follow-up** | UE 5.9 full release (not the preview) | Short comparison briefs: re-run key brief 001 tasks to measure what the 5.9 agent tooling changes |

## Why 5.8

- **It is what real work uses.** An upcoming work project runs agents (Claude / Codex) on an existing UE 5.8 game. Everything learned in the briefs transfers directly.
- **Agents already work on 5.8.** UE 5.8 ships an official MCP plugin, so the agent briefs do not need 5.9.
- **The plugin stack is validated on 5.8**, including packaged builds.
- **5.9 is a preview.** Epic built the 5.9 MegaJam Preview in about a week with a shorter QA pass than usual and is still cleaning up agent-written code. The full 5.9 release is expected in 2027.
- **The owned stack does not declare 5.9 yet.** On 2026-11-19, Fab's engine-version filter already offers 5.9 and 23 *Tools & Plugins* listings declare it, but none of the owned stack appeared among those checked. Of the wishlist, only **MeshBlend** and **MeshPack** declare 5.9. **Logic Driver Pro** lists 4.19–4.27 and 5.0–5.8 and states it supports the latest *official* release.

## When to start using 5.9

Start the 5.9 follow-up when **both** are true:

- UE 5.9 has its full (non-preview) release, and
- the plugins needed by the follow-up declare 5.9 on Fab (or compile and package cleanly in a test copy).

Migrating the main Sandbox is a separate decision: only if work projects move to 5.9 or the stack is fully supported there.

## Porting a plugin to a newer engine (when needed)

1. Work in a copy, never in the validated Sandbox.
2. Code plugins that ship source can often be recompiled, but this is unsupported until the vendor lists the version. Content / Blueprint-only assets usually open in newer versions; still test them.
3. Record the result per plugin (compiles / runs in PIE / packages) in `plugin-inventory.md`.
4. If it needs source changes, stop: maintaining a private fork of a commercial plugin is not worth it.

## Note on "last version before UE6"

Epic described 5.8 as the last *planned* major UE5 release, then announced 5.9 (preview now, full release in 2027). UE6 early access is targeted around the end of 2027. 5.9 is likely the last UE5 release, but Epic has not stated that formally, so plans should not depend on it.
