# Screen Space Fog Scattering — R&D Notes

## Purpose

This document captures the practical R&D conclusions for **Screen Space Fog Scattering (SSFS)** by Dmitry Karpukhin.

It is not a replacement for the vendor documentation. Exact CVars, version compatibility, and implementation details should be verified against the current documentation when needed.

Official documentation: https://dmkarpukhin.com/docs/ssfs/

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

## Current status

**Owned.**

Purchased in October 2026 during a discount period.
