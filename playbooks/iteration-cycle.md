# Iteration cycle — one brief every two weeks, built around alternating weeks

R&D here is personal work, done outside client hours. The Unreal machine is available **every other week**, so the cycle alternates between a **build week** (machine available) and a **write week** (no machine).

**Rule: one active brief at a time.** New ideas go to the candidates list in [../ROADMAP.md](../ROADMAP.md).

## The two-week cycle

```text
 BUILD WEEK (Unreal machine)            WRITE WEEK (no Unreal machine)
 ───────────────────────────            ──────────────────────────────
 Baseline → Build → Measure → Record    Decide → Write → Edit video → Publish
                                        + frame the NEXT brief
```

### Build week — everything that needs Unreal

| Step | Output |
|------|--------|
| 1. Baseline | Clean project copy or restore point; reference version if one exists |
| 2. Build | The smallest prototype that answers the question (Claude Code / Codex + Unreal) |
| 3. Measure | Numbers and observations written into brief sections 6–7 **the same day** |
| 4. Record | Clean takes of the final result + raw clips captured along the way |

**Before the build week ends:** everything that needs the machine must be captured (clips, screenshots, numbers, logs). The write week cannot go back for a missing shot.

### Write week — everything that does not need Unreal

| Step | Output |
|------|--------|
| 5. Decide | Brief sections 8–12: verdict, limits, reusable output, applying it at work, next question |
| 6. Ship | Video edited, post published, README tables updated (see [publishing.md](publishing.md)) |
| 7. Frame next | Next brief's sections 1–5 written, specs and prompts prepared in `assets/` |

Step 7 is what makes the next build week productive from the first hour: you sit at the machine with the question, the spec and the metrics already decided.

## Cadence

| Brief size | Cycle | Publication rhythm |
|------------|-------|--------------------|
| Small (fits one build week) | 2 weeks | 1 post every 2 weeks |
| Medium (needs two build weeks) | 4 weeks | 1 post per month |

Mixing both sizes gives roughly **one post every 2–4 weeks**. The minimum to keep the habit: **one post per month**.

If a build week is lost (illness, family, deadlines), the cycle simply shifts. Do not compress two cycles into one.

## Time box rules

- A small brief gets **one build week**; a medium brief gets **two**. Decide in the framing step.
- If the build week ends without an answer: measure what exists and publish a partial result, or extend by one build week **once**, writing down why.
- If it still does not converge: park it with a written reason. A parked brief is still valid evidence.

## Where things live

| Thing | Location |
|-------|----------|
| Public write-up | `briefs/NNN/BRIEF.md` |
| Specs, prompts, small scripts | `briefs/NNN/assets/` |
| Raw notes, logs, failed attempts | `lab/` |
| Reusable code | BertaDevKit (or the project repo), linked from the brief |

## Done means

- [ ] All sections of the brief are filled (or explicitly marked N/A)
- [ ] Time spent recorded
- [ ] 60–90 s video uploaded and linked
- [ ] Post published on nbertoa.com and linked
- [ ] Reusable output committed and linked
- [ ] Next brief framed (sections 1–5)
- [ ] Status updated in `README.md` and `briefs/README.md`
