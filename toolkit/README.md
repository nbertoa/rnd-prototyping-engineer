# Toolkit

The Unreal stack I prototype with, and how I decide what to add to it.

| File | Content |
|------|---------|
| [capability-map.md](capability-map.md) | What each system in the stack is responsible for, overlaps, gaps, integration experiments |
| [plugin-inventory.md](plugin-inventory.md) | Owned plugins, wishlist, purchase status and the purchase rule |
| [engine-versions.md](engine-versions.md) | Which engine version to use for what (5.8 stable stack vs 5.9 Preview AI track) and when to migrate |

**Guiding rule:** Unreal native → mature existing solution → integrate / extend → build custom only when justified. Brief [002](../briefs/002-plugins-in-the-ai-era/BRIEF.md) revisits this rule now that agents can build some tools on demand.

Detailed integration logs and annotated vendor manuals live in [`../lab/research/plugins/`](../lab/research/plugins/).
