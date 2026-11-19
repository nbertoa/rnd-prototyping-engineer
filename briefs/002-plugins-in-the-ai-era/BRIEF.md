# Brief 002 — Buy, build or let an agent build it: how should plugin decisions change?

| | |
|---|---|
| **Status** | Planned |
| **Engine** | UE 5.8 / 5.9 |
| **Size** | Small (1 build week; mostly analysis, writing can start in a write week) |
| **Time spent** | — |
| **Video** | — |
| **Post** | — |
| **Reusable output** | A buy / build / agent-build decision rule, applied to my own plugin inventory |

## 1. Question

With agents that can write C++ and edit Blueprints, which kinds of Unreal plugins are still worth paying for, and which have become cheaper to build on demand?

## 2. Why it matters

Fab runs frequent large sales, and every developer with a wishlist faces this question. UE 6 is planned for early access around the end of 2027, so UE5 plugins still have years of useful life, but their value is shifting.

For me: it turns my purchase rule (`toolkit/plugin-inventory.md`) into a tested, public decision framework.

## 3. Hypothesis

Plugins fall into three groups:

| Group | Examples | Agent impact |
|-------|----------|--------------|
| **Wrappers and glue** | Steam wrappers, simple save systems, localization automation | High: an agent can build the part I need |
| **Workflow and editor tools** | Blockout tools, graph formatting | Medium: depends on how much UI is involved |
| **Deep, battle-tested tech** | Fluid simulation, GPU terrain, mesh blending, mature camera systems | Low: the value is years of testing and edge cases |

A fourth axis matters: **how well agents can work with the plugin**. Plugins built on custom graph types or binary assets may be harder for agents to edit than plain C++ APIs.

## 4. Setup

- My owned stack and wishlist (`toolkit/plugin-inventory.md`) as the dataset.
- One small spike: ask an agent to build a minimal version of one "wrapper" plugin feature (for example, a basic save/restore of selected actors with `USaveGame`) and time it.

## 5. Method and metrics

- Classify every plugin in the inventory by group and by agent-friendliness.
- For the spike: time to a working minimal version, interventions, what was missing compared with the commercial plugin.

## 6. Result

—

## 7. What worked / what did not

—

## 8. Verdict and recommendation

Expected output: an updated purchase rule in `toolkit/plugin-inventory.md`.

## 9. Limits

Opinion-heavy by nature. The spike covers one feature only; a commercial plugin's real value (edge cases, support, updates) is hard to measure in a few hours.

## 10. Reusable output

The decision rule and the classified inventory.

## 11. Applying it at work

Apply the decision rule before proposing any plugin purchase on a project, and state in proposals which parts an agent can build instead.

## 12. Next question

Which owned plugins expose APIs that agents can actually call? That feeds brief 003.

---

### Video idea

Screen-recorded talk over the classification chart, then a 20-second timelapse of the agent spike with the time and the missing features on screen.
