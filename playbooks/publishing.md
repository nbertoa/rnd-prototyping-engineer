# Publishing — from brief to nbertoa.com

One brief produces several pieces of content. The brief is the source; everything else is derived from it.

## Pipeline

```text
BRIEF.md (source of truth)
   ├── Blog post on nbertoa.com   (full write-up, 800–1500 words)
   ├── Video (60–90 s)            (embedded in the post)
   ├── LinkedIn post              (3–6 lines: question, number, verdict, link)
   └── Repo README tables         (status + links)
```

## Blog post structure

Derived from the brief, in this order:

1. **The question and why it matters now** (sections 1–2)
2. **The result first**: the number and the decision, with the video embedded
3. **How it was tested** (sections 4–5, shortened)
4. **What worked / what did not** (section 7)
5. **Limits** (section 9). This is what makes it credible, so do not skip it
6. **What I am reusing** (section 10), linking to BertaDevKit or the repo

Category on nbertoa.com: **R&D Briefs**. Tags: engine version, main systems involved, `ai-agents` when relevant.

## Tone

- Engineering voice: claims backed by what was measured, honest ranges, explicit limits.
- No hype words ("revolutionary", "game-changer"). The numbers carry the message.
- Write for a lead engineer or technical director who has five minutes.

## Cadence

Target: **one published brief every two weeks**, or at least one every month. A short, finished brief beats a long, pending one.

## Before publishing

- [ ] Nothing from client work: code, assets, names, requirements, screenshots
- [ ] Third-party plugin content shown only as normal usage (no redistributed assets or source)
- [ ] Engine version and plugin versions stated
- [ ] Links between brief, post and video all work
