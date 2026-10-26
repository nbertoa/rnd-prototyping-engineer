# Ultra Dynamic Sky (UDS) — R&D integration notes

## Authoritative documentation — start here

- **Official documentation index, organized by UDS release:** https://www.ultradynamicsky.com/
- **Complete primary manual studied on 2026-10-26:** user-supplied `Ultra Dynamic Sky 9.7 Documentation.pdf`, **139 pages**. Technical coverage of all sections: [annotated version 9.7 reference with page numbers](ultra-dynamic-sky-9-7-complete-reference.md).
- **Official 9.7 vendor manual to consult when accessible:** https://www.ultradynamicsky.com/Documentation/V9/9-7 (PDF supplied by user is the verified content source; web address not independently fetched for this update).
- **Official 9.4 site previously used before receiving 9.7 PDF (historical only):** https://www.ultradynamicsky.com/Documentation/V9/9-4
- **In Unreal Editor:** select the `Ultra_Dynamic_Sky` actor and use its **UDS Documentation** button to open version-matched docs; consult property tooltips.
- **Core 9.7 pages for two showcase skies:** setup/modes pp.1–6, clouds pp.6–10, lights/exposure pp.10–14, time/sun/moon pp.14–20, fog/color pp.21–25, stars/sky glow pp.35–37, Configuration Manager pp.39–41, scalability pp.52–55, troubleshooting pp.120–130.

**Version caveat:** the **PDF version is 9.7**, but the **actual installed UDS version remains unverified**. Confirm it in Unreal before assuming exact property names or compatibility; 9.4 is no longer the primary documented source.

## Current visual experiment — two authored sky levels (2026-10-26)

**User objective:** build two **separate** levels from the tested `L_EnvironmentTest` baseline: `L_Sky_Sunset` (dramatic warm sunset) and `L_Sky_ClearNight` (clean dark sky with bright stars and restrained moonlight). These are **fixed environment looks**, not an automated day/night cycle. Keep the original baseline and current Chameleon level untouched.

**Detailed implementation and validation plan:** [Sunset + Clear Night UDS level study](../../projects/uds-sunset-clear-night.md).

**Source status updated (2026-10-26):** the user's **entire 139-page 9.7 PDF has now been read/reviewed**, including UDS, UDW, troubleshooting and architecture. See [complete reference](ultra-dynamic-sky-9-7-complete-reference.md). The revised [sunset/night implementation plan](../../projects/uds-sunset-clear-night.md) is grounded in that manual. Installed asset version, final visual parameters and two maps are still **pending actual validation in Unreal**.

**Built-in tools before custom development:** UDS already provides Time of Day, Stars, Moon, Sky Glow, Volumetric Clouds, Exposure, and a Configuration Manager for save/reapply of configurations. Prefer it to custom BertaDevKit sky-preset tooling for now. Compare SSFS as a complementary effect, not the sky source.

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
- The original packaged executable validates the **pre-environment Third Person baseline**.
- **New packaged build (2026-10-19):** user reports it **worked perfectly** after the UDS/SSFS environment changes. Mark the packaged-build gate **passed by user report**; exact executable settings and SSFS on/off comparison in the packaged version are not separately documented.
- `Project Settings → Maps & Modes → Game Default Map = L_EnvironmentTest` was requested as preparation, but the exact saved setting has not been separately confirmed.
- **Restore point:** the full-project backup `02_Environment_UDS_SSFS` was confirmed completed on 2026-10-19 (per later Sandbox project tracking). Keep it intact during the new sunset/night level experiment.
- Profile performance/scalability before calling the look production-ready: the UDS manual warns that Unreal's low/medium Shadow Quality scalability may disable volumetric fog, which makes Editor vs packaged settings relevant.

## Workflow reminder

When revisiting: use the **9.7 PDF-derived reference**, then confirm the **version-matched** in-editor UDS documentation, cross-check asset properties in this project, then propose **one change at a time**, validate and document actual result. Do not move straight to tuning SSFS intensity before the base fog is perceptible.
