# Ultra Dynamic Sky — Two sky-focused Sandbox levels

**Recorded:** 2026-10-26  
**Status:** planned; Unreal Editor changes, numerical tuning, screenshots and packaged-build validation **not yet performed**.  
**Engine:** Unreal Engine 5.8 Sandbox Prototype (user-reported setup).  
**Scope:** two **separate Unreal levels**, one warm dramatic sunset and one clear star-filled night; **not** a day/night cycle.

## Context and source integrity

- Sandbox already has an **Editor-tested and packaged** environmental baseline in `L_EnvironmentTest`, containing an `Ultra_Dynamic_Sky` actor, its built-in `Height Fog` component, and Screen Space Fog Scattering (SSFS). See [UDS setup](../research/plugins/ultra-dynamic-sky.md) and [SSFS setup](../research/plugins/screen-space-fog-scattering.md).
- Preserve `L_EnvironmentTest`, `L_ChameleonTest`, and the full-project restore point `02_Environment_UDS_SSFS`; do not edit the baseline in place.
- The user reports adding a comprehensive **Ultra Dynamic Sky PDF** to this ChatGPT project's sources. **Its bytes were not exposed in the accessible source/file set on 2026-10-26**, so its contents have **not** been verified or read. Do not label any settings below as coming from that PDF. Revisit it when available.
- Verified external reference used for this draft: [official Ultra Dynamic Sky v9.4 documentation](https://www.ultradynamicsky.com/Documentation/V9/9-4), which explains Time of Day, cloud modes, stars, sky glow, auto exposure, configuration saving and runtime/rendering modes. **The installed UDS version has not been confirmed.** Use the **UDS Documentation** button on the placed actor to check the manual that matches the installed version before tuning. Do not assume the fields are identical across releases.

## Scene design targets

### Level A — `L_Sky_Sunset`

**Creative target:** a large, expressive late-afternoon / sunset sky with a low sun near the horizon, warm gold/amber/pink highlights, cool contrasting blue/purple higher in the sky, distinct cloud silhouettes and long shadows. Strong visuals **without sacrificing readability** of ground/character.

**Proposed starting controls (hypotheses to verify in Editor, not finished presets):**

1. Duplicate `L_EnvironmentTest` to `L_Sky_Sunset`. Keep the existing `Ultra_Dynamic_Sky` actor and its components; do **not** add parallel Unreal `DirectionalLight`, `SkyLight`, `SkyAtmosphere`, `ExponentialHeightFog` or `VolumetricCloud` actors unless a specific verified need arises.
2. `Basic Controls → Animate Time of Day`: disable the ongoing animation in its named `Animate Time of Day` category for a **fixed** visual state. Confirm `Simulation → Use System Time` is not overriding the authored time.
3. `Basic Controls → Time of Day String`: try **17:30–18:30** as an exploratory *clock* window, then move time until the sun is **visibly close to the horizon**. No clock time guarantees a sunset: season, coordinates, sun simulation and UDS sun-path settings affect the result.
4. `Sky Mode`: begin with **Volumetric Clouds**, since their depth, shadows and light interaction can deliver the desired scale; keep **Game / Real-time** project mode for the playable prototype. Compare the lighter Static Clouds alternative only if frame time justifies it.
5. Increase/decrease **Cloud Coverage** to make a few large, readable cloud masses with holes that expose the lit horizon; tune **Sun Yaw / camera composition** only as needed. If **Simulate Real Sun** is enabled, the simulation overrides ordinary Sun Yaw/Pitch/Vertical Offset settings.
6. Start from the existing fog/SSFS baseline but **re-evaluate** its previously doubled `Base Fog Density`. Indoor visibility was the old goal; a majestic open sunset may need **lower** density and restrained SSFS. Compare `r.SSFS 0` and `r.SSFS 1` after the base sky/fog is right.
7. If the sky looks flattened or blown out, inspect **Exposure** settings on UDS and existing post-process volumes before adding grading. The objective is saturated atmosphere with visible highlights, not a uniformly orange image.

**Acceptance:** low sun visible in intentional composition; warm horizon vs cool upper sky; layered/cloud shadow depth; legible player and scene; steady fixed-time look in PIE.

### Level B — `L_Sky_ClearNight`

**Creative target:** an expansive, clean deep-blue/near-black night sky, plentiful crisp stars, optional readable moon and subtle silver-blue illumination. It must unmistakably read as **night**, not daytime tinted blue.

**Proposed starting controls (hypotheses to verify in Editor, not finished presets):**

1. Duplicate the **same baseline** `L_EnvironmentTest` to `L_Sky_ClearNight` instead of duplicating the sunset map; this avoids carrying untracked sunset tuning forward.
2. Disable automatic **Animate Time of Day** and any system-time override. Try **22:00–23:00** in `Time of Day String` and visually confirm that the sun is below the horizon; refine by inspecting the actual result.
3. For the first test use `Sky Mode = No Clouds`, or volumetric clouds with **minimal Cloud Coverage** if a few moonlit silhouettes are aesthetically valuable. Clear stars and wide open sky take priority.
4. In `Stars`, tune star **intensity, color and twinkling** with restraint. Optionally test `Simulation → Simulate Real Stars`: per v9.4 documentation this switches from default tiled stars to a higher-resolution 360° star map. It does **not** require realistic Sun/Moon simulation as a package.
5. In `Sky Glow`, keep **Night Sky Glow** subtle to preserve contrast; use little/no **Light Pollution** for a remote, clear-sky appearance. Tune the `Moon` controls/phase and moonlight to silhouette scenery instead of over-brightening the entire level.
6. Avoid reusing the old heavy, intentionally visible indoor fog uncritically: reduce `Fog`/base fog density if they wash out the star field or horizon. Keep SSFS only where it supports moonlight diffusion; compare on/off.
7. Diagnose apparent full brightness/darkness through `Exposure` and the map's Post Process Volume. Avoid compensating for an exposure conflict by arbitrarily maximizing sky or moon intensity.

**Acceptance:** deep dark sky with recognizable stars, no heavy cloud blanket, ground and character readable through deliberate moon/ambient balance, retained night contrast and stable PIE exposure.

## Recommended workflow — strictly incremental

1. **First action only:** duplicate `L_EnvironmentTest` as `L_Sky_Sunset`; open it and confirm the existing UDS + SSFS setup is intact. Do not change any sky properties yet.
2. Disable time animation and set/adjust **Time of Day** until the sun reaches the desired elevation. Capture a screenshot from a fixed camera.
3. Tune cloud coverage/composition; compare before/after. Only then adjust fog/SSFS and exposure, one control group at a time.
4. When the sunset look is acceptable, capture Details settings or save a full UDS configuration with the **built-in Configuration Manager** (UDS toolbar dropdown; version permitting). Don't create a BertaDevKit preset system for this first-use experiment.
5. Duplicate **the baseline** into `L_Sky_ClearNight` and repeat the same isolated tune-and-validate process (time → stars/sky glow/moon → fog → exposure).
6. Save screenshots of **both levels**, ideally same camera angles, viewport with **Game Settings** enabled, and record actual UDS/UE/SSFS versions and values in this document once measured.
7. Run **PIE** in both maps; perform a packaged-build checkpoint after both visual states are approved; capture scalability and frame timings before calling either look ready.

## Recording template (complete after hands-on tests)

| Check | Sunset | Clear night |
|---|---|---|
| Unreal level exists | Pending | Pending |
| UDS version confirmed | Pending | Pending |
| Fixed Time of Day verified | Pending | Pending |
| Clouds / stars / moon reviewed | Pending | Pending |
| Fog and SSFS A/B | Pending | Pending |
| Exposure verified in PIE | Pending | Pending |
| Screenshots | Pending | Pending |
| Packaged build | Pending | Pending |
| Runtime performance | Pending | Pending |

## Relevant vendor features, not custom code

- `Time of Day String` accepts values such as `15:45`, while `Time of Day` is the basic numeric control.
- `Animate Time of Day` is separate from the static clock setting, and real-sun simulation changes how timing works.
- `Sky Mode`: Volumetric Clouds, Static Clouds, 2D Dynamic Clouds, No Clouds.
- `Color Mode`: Sky Atmosphere or Simplified Color; begin with the installed/current setting rather than switching arbitrarily.
- `Exposure` controls include bias, metering and brightness range; existing PPV overrides can conflict.
- `Stars`, `Sky Glow` and `Moon` each have built-in night-sky controls.
- **Configuration Manager** can save/reapply full UDS configurations across levels and projects. Evaluate this before building anything custom.
- Leave `Project Mode = Game / Real-time` for playable tests. `Cinematic / Offline` is intended for offline renders and can significantly hurt game performance.
- Official vendor documentation: https://www.ultradynamicsky.com/Documentation/V9/9-4

## Design decision

Two **separate levels with fixed sky states** are the lowest-risk way to compare authored sunset vs clear night in the existing Sandbox. No new time-of-day framework, global manager, BertaDevKit subsystem, or weather actor is needed until an actual reusable need is demonstrated. Keep the third-party asset untouched.
