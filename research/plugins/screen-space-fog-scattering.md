# Screen Space Fog Scattering — R&D Notes

## Purpose

This document captures the practical R&D conclusions for **Screen Space Fog Scattering (SSFS)** by Dmitry Karpukhin.

It is not a replacement for the vendor documentation. Exact CVars, version compatibility, and implementation details should be verified against the current documentation when needed.

**Official documentation (quickstart, CVar reference, compatibility, troubleshooting, changelog):** https://dmkarpukhin.com/docs/ssfs/

**Official product page:** https://dmkarpukhin.com/ssfs/

**Documentation verification (2026-10-19):** The official docs list SSFS version 1.32 as compatible with Unreal Engine 5.8. The exact version installed in this Sandbox has **not** been checked. Check the actual installed plugin version if UI/options differ.

---

## R&D role

**Primary role:** improve the perceptual quality of existing Unreal fog by adding screen-space light scattering.

SSFS is a presentation / atmosphere accelerator rather than a separate weather or fog system.

It complements:

- **Ultra Dynamic Sky** — weather, sky, time of day, atmospheric state;
- **Unreal Exponential Height Fog / Volumetric Fog** — underlying fog medium;
- **Chameleon Post Process** — perception, stylization, visual states;
- lighting and cinematic workflows.

Conceptually:

```text
UDS / Unreal atmosphere
        ↓
Exponential Height Fog
        ↓
Screen Space Fog Scattering
        ↓
Improved light diffusion / denser perceptual fog
        ↓
Chameleon / camera / cinematic grading if needed
```

---

## Existing-level workflow

If a level already uses **Exponential Height Fog**, SSFS does not require replacing that setup.

1. Install the plugin for the target engine version.
2. Enable **Screen Space Fog Scattering** in `Edit -> Plugins`.
3. Restart Unreal Editor.
4. Open a level that already contains an `ExponentialHeightFog` actor or a Blueprint/system containing an `ExponentialHeightFogComponent`.
5. The scattering effect should apply automatically in fogged regions.

The effect intensity is driven partly by the existing Exponential Height Fog settings, so the first tuning pass should remain on the existing fog rather than rebuilding the level around SSFS.

This makes it particularly suitable for existing prototype levels and for systems such as Ultra Dynamic Sky when their fog ultimately uses Unreal's Exponential Height Fog component.

---

## First controls to know

Useful console variables from the current SSFS documentation:

```text
r.SSFS 1
```

Enable / disable the effect. Useful for A/B comparisons.

```text
r.SSFS.Intensity 1.0
```

Global multiplier for the scattering effect, scaled by Exponential Height Fog.

```text
r.SSFS.Radius 0.9
```

Controls the progressive radius / spread of the scattering. Recommended vendor range is roughly `0.8–0.99`.

```text
r.SSFS.QualityPreset 2
```

Quality preset:

- `0` Low
- `1` Medium
- `2` High
- `3` Epic

```text
r.SSFS.PassAmount 8
```

Number of scattering passes. Vendor guidance is approximately 8 for Full HD, 9 for QHD, and 10 for 4K.

```text
r.SSFS.SkyAtmosphere 0
```

Optional Sky Atmosphere contribution. Disabled by default.

Additional controls exist for Volumetric Fog integrated light scattering, Heterogeneous Volumes, texture format, Volumetric Clouds, and filtering.

---

## Recommended first test

Do not tune multiple systems at once.

For an existing foggy level:

1. capture the current look;
2. enable SSFS with defaults;
3. toggle `r.SSFS 0/1` for a direct A/B comparison;
4. if the effect is too strong, adjust `r.SSFS.Intensity` before changing the original fog;
5. only then adjust the underlying Exponential Height Fog if the scene itself needs denser / lighter fog;
6. profile the target platform before treating the look as final.

This keeps the distinction clear between **fog design** and **scattering enhancement**.

---

## Integration decision

Treat SSFS as a low-friction visual enhancement.

Do not wrap it in BertaDevKit unless a repeated need emerges for runtime presets, diagnostics, or automatic quality/platform switching.

For now:

- keep vendor plugin untouched;
- use Unreal / UDS for actual fog state;
- use SSFS to enhance scattering;
- use Chameleon for authored perception / stylization states.

---

## Sandbox Prototype — validated UDS + SSFS workflow (2026-10-19)

**Project:** UE 5.8 Blank C++ Sandbox with stock Third Person Blueprint Feature Pack. **Level:** `L_EnvironmentTest` copied from `ThirdPersonMap`. [UDS source/steps](ultra-dynamic-sky.md) and [Sandbox progress](../../projects/sandbox-prototype.md).

- Existing sky/light/fog actors on the duplicated map were removed and `Ultra_Dynamic_Sky` was placed in the map.
- UDS' fog component is named **`Height Fog`** (confirmed in the installed Blueprint's Components panel). SSFS works on that system without adding a second Exponential Height Fog actor.
- Initial test `r.SSFS 0` vs `r.SSFS 1` showed a subtle but visible change. This **verified the effect operates in the Editor**.
- The enclosed Third Person playground had almost no visible near-camera fog. In UDS, `Fog = 1.0`, `Use Volumetric Fog` was already enabled, and `Min Fog Start Distance = 0`, `Max Fog Start Distance = 0` made only a small improvement.
- **Doubling UDS `Base Fog Density` from the original value** made fog clearly visible indoors. The exact resulting numeric value was **not recorded**.
- With the denser underlying fog, the same `r.SSFS 0` / `r.SSFS 1` A/B showed a **perceptible difference**, confirmed by the user.
- `r.SSFS.Intensity 2.0` was suggested/explored, but the final **persistent value is unverified**. Do not treat it as a required setting or recorded final preset.
- **Validated:** visually in Editor. **Pending:** package the new environment level and validate the bundled executable. The initial Third Person package (before environment edits) had already passed; do not misattribute that pass to SSFS.

**Practical diagnostic:** if SSFS changes little, first verify there is enough **underlying Unreal/UDS fog in the visible depth range**. SSFS enhances light scattering; it does not replace fog-density controls. Changing many variables at once obscures the cause.

## Current status

**Owned.**

Purchased in October 2026 during a discount period.
