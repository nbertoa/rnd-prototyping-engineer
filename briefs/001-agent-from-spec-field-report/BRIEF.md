# Brief 001 — What can an agent rebuild from a spec in a real UE 5.8 project?

| | |
|---|---|
| **Status** | Planned |
| **Engine** | UE 5.8 (copy of the validated Sandbox), official Unreal MCP plugin |
| **Size** | Medium (2 build weeks) |
| **Time spent** | — |
| **Video** | — |
| **Post** | — |
| **Reusable output** | Spec templates for delegating Unreal tasks to agents + a "delegate / supervise / do it myself" table |

## 1. Question

Given a written spec, which Unreal tasks can a coding agent (Claude Code / Codex through the official Unreal MCP plugin) complete inside a real UE 5.8 project with a commercial plugin stack, and how much human correction does each one need?

## 2. Why it matters

UE 5.8 ships an official MCP plugin, UE 5.9 expands the agent toolsets and UE6 is planned around AI integration. Most studios, though, will run agents on **existing UE 5.x projects full of third-party plugins**, not on clean samples. Most public material is demos on empty projects, not measurements on real ones.

For me: it decides which parts of real prototyping work (including an upcoming work project on UE 5.8 with Claude / Codex) I can delegate.

## 3. Hypothesis

- Data and config tasks (Data Assets, Data Tables, Gameplay Effects, input mappings) succeed with few interventions.
- Blueprint graph logic succeeds for short linear graphs and degrades with branching, latent nodes and third-party node types.
- Cross-asset wiring (character ↔ controller ↔ GameMode ↔ components) is where most interventions happen.

## 4. Setup

Engine track: UE 5.8 stable stack (see `toolkit/engine-versions.md`). A short UE 5.9 follow-up is planned once 5.9 has its full release.


Reuse tasks I already solved by hand in the UE 5.8 Sandbox, so there is a known-good reference and a real time baseline (see `lab/projects/sandbox-prototype.md`):

| Task | Reference I already built by hand |
|------|----------------------------------|
| T1 — Jump as a Gameplay Ability with a stamina cost | `GA_Jump` + `GE_Jump_Cost` + `Activation Blocked Tags = InAir` |
| T2 — Stamina regeneration | `GE_Stamina_Regen` (Infinite, periodic) |
| T3 — Sprint with stamina drain and forced stop at zero | `GA_Sprint` + `GE_Sprint_Drain` |
| T4 — Airborne state tags from CharacterMovement | `On Movement Mode Changed` → loose tags |
| T5 — Data-driven camera presets cycle | UGC Data Assets Near / Medium / Far |

Out of scope: packaging, multiplayer, performance.

## 5. Method and metrics

For each task:

1. Write a spec in `assets/specs/Tn.md` (requirements + acceptance checks, no implementation hints).
2. Give the spec to the agent in a fresh copy of the baseline project.
3. Log every intervention: clarification, correction, manual fix in the editor.
4. Verify against the acceptance checks in PIE.

Metrics per task: success (yes / partial / no), number of interventions, wall-clock time (range), defects found after the agent reported "done".

## 6. Result

—

## 7. What worked / what did not

—

## 8. Verdict and recommendation

Expected output: a table classifying task types as **delegate**, **delegate with supervision** or **do it myself**, for UE 5.8.

## 9. Limits

One project, one person, a handful of tasks, UE 5.8 tooling. The results describe this setup in late 2026, not agents in general; UE 5.9 tooling may change them.

## 10. Reusable output

- `assets/specs/` — spec format that worked best, reusable for client work
- Delegation table, reused in brief 003 and in daily work

## 11. Applying it at work

Use the delegation table in client prototyping: hand data/config tasks to the agent with a spec, supervise graph logic, keep cross-asset wiring manual until brief 003 proves otherwise.

## 12. Next question

If the agent fails mostly at verification, can it check its own work? → Brief 003.

---

### Video idea

Split screen: the spec on the left, the editor on the right as the agent builds T1. Counter overlay: interventions / minutes. End on the delegation table.
