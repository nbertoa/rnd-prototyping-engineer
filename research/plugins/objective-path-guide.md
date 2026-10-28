# Objective Path Guide — Technical Reference and R&D Integration Notes

**Status:** Owned / purchased — user-confirmed on 2026-10-27. **Installed, enabled, PIE-tested and packaged-build-tested status:** not yet verified. Do not infer runtime compatibility from ownership.

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

## 7. Verification gates

- [x] User confirmed purchase/ownership (2026-10-27).
- [x] Vendor's complete 10-page PDF reviewed and its component names, navigation settings, VFX controls, and reveal/hide modes extracted.
- [ ] Verify actual asset version and compatibility in target Unreal Engine (including UE 5.8, if used).
- [ ] Confirm added/installed state in the chosen project and correct NavMesh coverage.
- [ ] Test `Start Guidance To`, `Start Guidance To Via`, `Stop Guidance`, `Is Guidance Active?` in PIE.
- [ ] Test curved path around obstacles, deviation updates, arrival behavior, and fast repeated activation.
- [ ] Test fade/reveal timing with a project-owned timer.
- [ ] If needed, test a truly custom Niagara and document the required adapter/parameter changes.
- [ ] Profile performance on representative path sizes and verify packaged build.
- [ ] If multiplayer is in scope, validate client-side navigation and multiplayer behavior separately.

**Decision:** preferred acquired solution for a **temporary, aesthetically pleasing in-world navigation trail**. Start with vendor-supplied visuals and native integration; expand only when a concrete need is validated.
