# Objective Path Guide — Technical Reference and R&D Integration Notes

**Status:** Owned / purchased (user-confirmed 2026-10-27) and **integrated into the user's current Unreal project (user-confirmed 2026-10-28)**. Project name, precise Unreal version and packaging state not yet supplied. Startup guidance has run successfully with a brief delay, but an initial visual artifact remains; this is **functional integration with a known issue**, not a fully validated or production-ready build.

**Fab:** https://www.fab.com/listings/1621772d-0a63-4415-841c-0ccd0c1d82c8

**Primary source:** vendor's 10-page `ObjectivePathGuide.pdf` / *Objective Path Guide: Documentation*, supplied in project sources on 2026-10-27 (PDF metadata: 2026-09-05). Refer to the original PDF for screenshots and exact Details-panel spelling; this note is an indexed implementation reference, **not** a replacement for it.

**Vendor demo:** https://youtu.be/W8LFTMxudrs

**R&D role:** visually polished, in-world route guidance using NavMesh-based paths and Niagara effects; a player-triggered, temporary glowing trail toward a destination. Distinct from UI minimap, POI marker and map route rendering.

## 1. Documented minimum integration (PDF pp. 2–5)

1. Place and correctly size a `NavMesh Bounds Volume` to cover navigable areas. Press `P` to inspect the green navigation surface; use `NavMesh Modifier Volume` to exclude interiors or surfaces that should not be navigable.
2. Add **Guide Component** to the player Character Blueprint.
3. Call **Start Guidance To** with a target Actor or target Location. When both are supplied, the target Actor takes precedence.
4. Alternatively, call **Start Guidance To Via** with an ordered `Via` array of **BP_Waypoint** actors to force intermediate route points.
5. Call **Stop Guidance** to hide the spline and visual effects; query **Is Guidance Active?** for current state.

The Guide Component Details sections are **Guide Navigation** and **Niagara Customisation** (PDF p. 4).

**Smoke-test first:** path around an obstacle to a fixed target, then deviation-based update, fade-out, and arrival cleanup. Do not start with custom Niagara or cross-plugin integration.

## 2. Actual visual architecture (PDF p. 6)

- **Spline:** navigation-guidance route, with a Niagara system visualizing the spline.
- **BP_Guide:** moving actor traveling along the spline.
- **Orb Niagara:** visual effect on `BP_Guide`.
- **Ground Niagara:** second visual effect on `BP_Guide`.

**Guiding Mode:** `Orb Guide`, `Spline Guide` or `Both`. The first uses `BP_Guide` with Orb and Ground effects; the second shows only the spline; the last combines both.

This is **a purpose-built guide effect**, not proof that arbitrary Niagara System assets can be plugged in without changing the provided logic or parameter bindings.

## 3. Guide Navigation settings (PDF pp. 7–8)

| Setting | Vendor-documented function |
| --- | --- |
| `Path Zoffset` | Vertical offset relative to ground. |
| `Clearance` | Separation from walls/obstacles. |
| `Path Tangent Limit` | Limits spline tangents: lower = more angular; higher = smoother; 0 = no limit. |
| `Update Path` | Recalculates if the player moves far enough from the spline; the travelled spline progressively disappears. |
| `Update Distance Trigger` | Player-to-spline deviation distance to initiate recalculation. |
| `Disappear on Arrival` | Hide spline at destination. |
| `Arrival Radius` | Proximity threshold used for arrival disappearance. |
| `Orb Waits For Player` | Orb may pause to wait for the player. |
| `Orb Max Distance Ahead` | Maximum lead distance while waiting is enabled. |
| `Navigation Filter Class` | Navigation query filter used to find path. |
| `Super Sample` / `Sample Distance` | More frequent spline sampling and sample interval for trajectory correction. |
| `Show Debug` | Spline construction diagnostics. |

**Scope of dynamic behavior:** documented recalc trigger is **player deviation from the spline**. Do not assume automatic recalc on arbitrary navmesh changes or moving goals without a dedicated test.

## 4. Niagara look and reveal/hide controls (PDF pp. 7–9)

**Spline** includes `Ribbon Color`/`Ribbon Emissive`, two sparkles colors and emissive multipliers, `Sparkles Life Time`, `Sparkles Amount`, `Sparkles Speed`/`Speed Deviation`, ribbon width, floating amplitude/frequency/speed, texture panning speed, number of ribbons and fog visibility. Particle and ribbon travel direction can be reversed by negative speed values where documented.

**Appearance/disappearance modes** are explicitly documented:

- `Spline Reveal Mode`: `Linear` (progressive reveal from player toward destination) or `Fade` (whole spline fades in).
- `Spline Reveal Speed`, `Spline Fade in Speed`.
- `Spline Disappear Mode`: `Linear` (progressive removal from player toward destination) or `Fade` (whole spline fades out).
- `Spline Hiding Speed`, `Spline Fade Out Speed`.

**Orb**: enable/disable Orb Niagara; rays and sparkles colors/emissive, sparkles amount, floating deviation amplitude/frequency, Z offset, scale and speed.

**Ground**: enable/disable Ground Niagara; ribbon/sparkles colors and emissive, sparkles amount.

**Temporary hint example (integration proposal, not a documented built-in timer):** input event → `Start Guidance To` → project-owned timer → `Stop Guidance`. Tune fade-out settings. Cancel/replace the timer when re-triggered, and separately test arrival and any repeated call behavior.

**Custom Niagara status: NOT CONFIRMED.** The PDF documents configurable Niagara effects and a three-part system; it does **not** document a public setting for replacing each built-in system with an arbitrary custom `NiagaraSystem`. Possible extension by editing the provided Blueprint/Niagara setup is an **engineering hypothesis**, not a supported drop-in integration claim. Test on a duplicate if needed.

## 5. Multiplayer and known troubleshooting (PDF pp. 6, 10)

- Vendor instruction for multiplayer: **Project Settings → Navigation System → Allow Client Side Navigation**. Every player sees **their own** path, not other players' paths; this is the documented intended behavior, not a verified replication solution. Assess security/performance implications before enabling for a networked production game.
- **Spline appears to cut through walls:** confirm obstacles are actually removed from navigable surfaces; the vendor recommends a `NavMesh Modifier Volume` where geometry has an unwanted interior NavMesh. A visually clipped line is not automatically a Niagara rendering bug.
- Long paths, packaging, UE 5.8 compatibility, platform performance and behavior with moving destinations require hands-on verification; the PDF does not provide benchmarks or a compatibility matrix.

## 6. Relationship to existing toolkit

- **Map System Pro:** map/minimap, markers, POIs, map-oriented route presentation. **Objective Path Guide:** in-world path visualization with spline/Orb/Ground Niagara and destination guidance. Complementary UI/world presentation layers; **no official direct interoperability is established**.
- **Unreal native Navigation System:** substrate for NavMesh and navigability; this asset packages navigation + presentation behaviors, not a replacement for native navigation.
- **BertaDevKit:** do not copy third-party code or create an adapter by default. Only consider optional, license-respecting integration if multiple projects need the same generic guidance interface.

## 7. User-reported project integration — 2026-10-28

**Evidence level:** user report during iterative Unreal Editor troubleshooting; the project/level name and engine version were not explicitly provided. **Do not equate this with the previously documented UE 5.8 Sandbox**, and do not assume a packaged Windows build.

### Implemented in the active project

1. NavMesh was already present in the level.
2. Added **Guide Component** to the player's Character Blueprint.
3. Set **Guide Navigation → Guiding Mode = Both**, with the intended combination of spline trail and moving orb.
4. Placed a **Target Point** in the level as destination (suggested label `TP_GuideDestination`; exact saved asset/actor name not verified).
5. Configured automatic activation at level start using **Level Blueprint → Event BeginPlay → Get Player Character → cast to project Character → Guide Component → Start Guidance To**, with the destination reference connected as **Target Actor**. This graph is based on the configuration steps used in the conversation; the exact currently saved node graph was not independently inspected.
6. Initial runtime error: **`Accessed None trying to read property PathSpline`** when starting guidance immediately at BeginPlay.
7. A **0.2-second Delay before starting guidance** was reported to make the path-guidance logic work. This indicates an **apparent startup-order/timing dependency**, not a confirmed root cause. During that interval, the user observed Niagara effects appearing **without a valid path**, so the visual artifact is **not confirmed fixed**.
8. Blueprint node discovery issue in this new project: with **Context Sensitive** enabled, the standard seconds-based `Delay` node was not shown; the user associated this behavior with **LE Extended Standard Library**. Disabling **Context Sensitive** exposed the native `Delay` node with **Duration (seconds)**. **Do not claim a proven engine/plugin incompatibility or that Unreal removed seconds-based Delay**; this is an observed context-sensitive search/filter collision or ambiguity.

### Current validation state

- **User-confirmed:** plugin acquired; added/integrated into this project; route guidance works with the startup Delay workaround; basic setup has a destination and NavMesh.
- **Open issue:** brief Niagara rendering without route during the ~0.2 s startup delay, and underlying `PathSpline` initialization ordering.
- **Not separately confirmed:** an entirely artifact-free first 0.2 s, whether the guide/orb waits correctly for the player, recomputation after deviating, arrival disappearance, `Start Guidance To Via`, custom Niagara replacement, a packaged executable, actual UE version, or long-path performance.

### Recommended next troubleshooting step (not performed)

Inspect the initialization sequence of the guide and Niagara components; ensure path construction / valid `PathSpline` comes **before** activating the relevant Niagara renderers. Prefer a deterministic readiness signal / validated reference over a fixed time delay for a production-quality setup. If the delivered asset only supports a fixed Delay, hide/deactivate the relevant visuals until the path is ready, then activate after `Start Guidance To`. **An earlier suggestion to disable `Auto Activate` on `BP_Guide` Niagara components was not confirmed tested and should not be recorded as a completed fix.**

---

## 8. Verification gates

- [x] User confirmed purchase/ownership (2026-10-27).
- [x] Vendor's complete 10-page PDF reviewed and its component names, navigation settings, VFX controls, and reveal/hide modes extracted.
- [ ] Verify actual asset version and compatibility in target Unreal Engine (including UE 5.8, if used).
- [x] User confirms plugin integrated into the current project and a NavMesh exists in its level (2026-10-28); exact project name, asset version, and navmesh coverage not independently verified.
- [x] User reports `Start Guidance To` works with a ~0.2 s startup delay; the initial Niagara-without-path artifact remains.
- [ ] Separately test `Start Guidance To Via`, `Stop Guidance`, `Is Guidance Active?` and verify exact behavior in PIE.
- [ ] Test curved path around obstacles, deviation updates, arrival behavior, and fast repeated activation.
- [ ] Validate fade/reveal controls and a project-owned timer if temporary hints are required.
- [ ] If needed, test a truly custom Niagara and document the required adapter/parameter changes.
- [ ] Profile performance on representative path sizes and verify packaged build.
- [ ] If multiplayer is in scope, validate client-side navigation and multiplayer behavior separately.

**Decision:** preferred acquired solution for a **temporary, aesthetically pleasing in-world navigation trail**. Start with vendor-supplied visuals and native integration; expand only when a concrete need is validated.
