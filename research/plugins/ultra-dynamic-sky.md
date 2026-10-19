# Ultra Dynamic Sky (UDS) — R&D integration notes

## Authoritative documentation — start here

- **Official documentation index, organized by UDS release:** https://www.ultradynamicsky.com/
- **Official UDS 9.4 manual consulted for this integration:** https://www.ultradynamicsky.com/Documentation/V9/9-4
- **In Unreal Editor:** select the `Ultra_Dynamic_Sky` actor and use the **UDS Documentation** button at the top of its Details panel to open the manual matching the installed asset, when available.
- **Relevant manual sections:** `Sky → Sky Setup → Adding Ultra Dynamic Sky to Your Level`, `Basic Controls`, `Fog → Fog Density` and `Fog → Volumetric Fog`.

**Version caveat:** The exact **installed UDS version is not recorded** for this Sandbox. UDS documentation is versioned (V6–V9). Do not assume that the 9.4 manual always matches the installed Blueprints; confirm in-editor names/tooltips before issuing step-by-step instructions. The 9.4 link records which documentation was consulted, not a verified installed version.

## Role in this Sandbox

UDS supplies sky, lighting, clouds and controls for fog/atmospheric state. Screen Space Fog Scattering (SSFS) enhances scattering of the underlying fog; it is not a substitute for the fog provided by UDS' `Height Fog` component.

For the companion SSFS integration and official docs, see [Screen Space Fog Scattering](screen-space-fog-scattering.md).

## Integration completed in UE 5.8 — 2026-10-19

**Project:** Sandbox Prototype, rebuilt as a Blank C++ Unreal Engine 5.8 project with the stock Third Person Blueprint Feature Pack.

**Baseline:** Stock Third Person ran successfully in Editor and in a packaged executable; the full-project backup `01_ThirdPerson_Baseline` was confirmed *before* making environmental changes.

**Working test level:** `L_EnvironmentTest`, a duplicate of `ThirdPersonMap` (original retained intact).

Procedure executed and user-confirmed:

1. Inspected stock level and confirmed it contained `DirectionalLight`, `SkyLight`, `SkyAtmosphere`, `ExponentialHeightFog` and `VolumetricCloud`.
2. Removed **those five actors only** in `L_EnvironmentTest`, retaining other level actors such as the PostProcessVolume.
3. Added `Ultra_Dynamic_Sky` Blueprint actor to the level at ground level, scale `1, 1, 1`.
4. Visually confirmed that sky, clouds and lighting work in the Editor.
5. Inspected the UDS Blueprint components and confirmed the fog component is named **`Height Fog`** in this installed asset.
6. To improve visibility of fog within the close-range/walled Third Person playground:
   - `Basic Controls → Fog` set to **`1.0`** for the test.
   - `Use Volumetric Fog` was **already enabled**; no change necessary.
   - `Fog Density → Min Fog Start Distance` and `Max Fog Start Distance` set to **`0`**. This gave only a slight perceptual increase.
   - `Fog Density → Base Fog Density` **doubled from the value previously present**. This produced clearly visible, denser fog in the enclosed scene, user-confirmed. **Exact numeric before/after values were not captured; do not invent them.**
7. With this denser fog, SSFS `r.SSFS 0` vs `r.SSFS 1` produced a **perceptible** difference (user-confirmed).

**Conclusion supported by testing:** UDS is functioning in the Editor, and fog becomes visible in close-range geometry after raising base density. The subtle indoor fog beforehand was not evidence that UDS or SSFS were broken. Only the doubled base density had a marked effect.

## Status and next validation gate

- UDS runtime visualization **verified in Editor**, in `L_EnvironmentTest`.
- The original packaged executable only validates the **pre-environment Third Person baseline**.
- A **new package including the atmosphere/fog level** was underway as of the latest 2026-10-19 user report; **outcome not yet reported**.
- `Project Settings → Maps & Modes → Game Default Map = L_EnvironmentTest` was requested as preparation, but **the saved setting has not been independently confirmed**.
- Verify new packaged executable launches the correct level and displays UDS fog + SSFS, then save a new restore point **only after the packaged test passes**.
- Profile performance/scalability before calling the look production-ready: the UDS manual warns that Unreal's low/medium Shadow Quality scalability may disable volumetric fog, which makes Editor vs packaged settings relevant.

## Workflow reminder

When revisiting: open the official **version-matched** UDS documentation first, cross-check asset properties in this project, then propose **one change at a time**, validate and document actual result. Do not move straight to tuning SSFS intensity before the base fog is perceptible.
