# Iteration cycle — one brief in 1–2 weeks of spare time

R&D work here happens in the gaps of client work. The cycle is designed so that a brief can be paused at any step without losing progress, and so that something publishable comes out even if the result is "not worth it".

**Rule: one active brief at a time.** New ideas go to the candidates list in [../ROADMAP.md](../ROADMAP.md).

## The six steps

| Step | Output | Typical time |
|------|--------|--------------|
| 1. Frame | `BRIEF.md` sections 1–5 filled: question, hypothesis, setup, metrics, time box | 1 h |
| 2. Baseline | Clean project copy or restore point, reference version if one exists | 0.5–1 h |
| 3. Build | The smallest prototype that can answer the question | 50–60% of the box |
| 4. Measure | Numbers and observations written into sections 6–7 while still fresh | 1–2 h |
| 5. Decide | Sections 8–11: decision, limits, reusable output, next question | 1 h |
| 6. Ship | Video, post, README tables updated (see [publishing.md](publishing.md)) | 2–3 h |

## Time box rules

- Set the time box in step 1 and track the time actually spent.
- At 100% of the box, stop building. Either measure what exists or extend **once**, writing down why.
- At 150%, park the brief with a written reason. A parked brief is still a valid entry.

## Recording as you go

Recording at the end is where most briefs die. Instead:

- Capture short clips during step 3 whenever something works for the first time (OBS replay buffer, or ShadowPlay / Instant Replay).
- Save them into `briefs/NNN/media/raw/` (git-ignored) with a one-line note of what they show.
- Step 6 then becomes editing, not re-creating.

## Where things live

| Thing | Location |
|-------|----------|
| Public write-up | `briefs/NNN/BRIEF.md` |
| Specs, prompts, small scripts | `briefs/NNN/assets/` |
| Raw notes, chat-derived logs, failed attempts | `lab/` |
| Reusable code | BertaDevKit (or the project repo), linked from the brief |

## Done means

- [ ] All 11 sections of the brief are filled (or explicitly marked N/A)
- [ ] Time spent recorded
- [ ] 60–90 s video uploaded and linked
- [ ] Post published on nbertoa.com and linked
- [ ] Reusable output committed and linked
- [ ] Status updated in `README.md` and `briefs/README.md`
