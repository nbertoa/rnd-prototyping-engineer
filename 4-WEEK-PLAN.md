# 4-Week Focus Plan

## Goal

Strengthen my R&D / Prototyping Engineer profile through real work, using **BertaDevKit as the primary reusable Unreal Engine R&D toolset** and improving **C++ architecture** through concrete problems rather than isolated exercises.

This plan should stay lightweight. The objective is evidence, not course completion.

---

## Week 1 — Inspect and Choose One Real Problem

### Objectives

- Review current BertaDevKit structure and recent work.
- Identify one real friction point, repeated problem, weak architectural area, or missing diagnostic that is worth improving.
- Check whether Unreal Engine or an existing plugin already solves it well enough before building anything.

### Questions

- What problem is actually being solved?
- Who benefits from the solution?
- Is it recurring enough to justify reuse?
- Does it belong in BertaDevKit or only in a specific project?
- Can Unreal native functionality solve it?
- Is there a mature plugin worth buying or integrating instead?

### Output

A short problem statement containing:

- context;
- requirements;
- constraints;
- alternatives considered;
- initial recommendation.

---

## Week 2 — Prototype and Validate

### Objectives

- Build the smallest useful prototype for the selected problem.
- Prefer simple Unreal-native approaches first.
- Use C++ and Blueprints where each makes sense.
- Use AI actively for API research, alternative exploration, debugging, and code review.

### C++ focus

Pay particular attention to:

- ownership and lifetime;
- module boundaries;
- dependencies;
- composition vs inheritance;
- API surface;
- keeping prototype complexity proportional to the problem.

### Output

A working spike or prototype plus notes on:

- what worked;
- what failed;
- what assumptions were validated or rejected;
- remaining risks.

---

## Week 3 — Decide: Tool, Integrate, Buy, or Stop

### Objectives

Use the prototype evidence to decide what should happen next.

Possible outcomes:

- keep the solution project-specific;
- promote it into BertaDevKit;
- integrate or extend an existing plugin;
- buy a mature plugin instead;
- abandon the approach because it is not worth maintaining.

### If promoted to BertaDevKit

Review:

- plugin/module placement;
- public API boundaries;
- naming;
- diagnostics;
- documentation;
- reuse outside the original problem.

### Output

A documented engineering decision explaining the chosen direction and tradeoffs.

---

## Week 4 — Explain and Package the Learning

### Objectives

Turn the technical work into evidence that can be reused professionally.

Create at least one concise artifact:

- technical note;
- case study;
- architecture diagram;
- short PPT;
- demo video or script.

### Structure

1. What was the problem?
2. What were the requirements and constraints?
3. What alternatives were considered?
4. What was prototyped?
5. How does it work?
6. Why was this approach chosen?
7. What are the limitations?
8. What should happen next?

### Output

One finished communication artifact and one entry capturing what should be reused in future R&D work.

---

## Continuous Rules

### Buy / Integrate Before Build

Evaluate in this order:

1. Unreal Engine native solution.
2. Mature existing plugin or library.
3. Buy vs build tradeoff.
4. Integrate or extend an existing solution.
5. Custom development only when justified.

### BertaDevKit

BertaDevKit is infrastructure, not a dumping ground.

Promote something into it only when reuse, diagnostics, integration, or workflow value is clear.

### AI

Use AI continuously for research, hypothesis generation, API exploration, debugging, architecture critique, documentation, and communication, while keeping engineering judgment and verification explicit.

### Evidence over study

Prefer one real prototype, tool improvement, decision record, case study, or demo over completing generic coursework without applied evidence.

---

## Success Criteria After 4 Weeks

At the end of the cycle I should have:

- investigated one real Unreal R&D problem;
- evaluated native, plugin, integration, and custom-build options;
- produced a working prototype or useful technical conclusion;
- improved or deliberately not changed BertaDevKit based on evidence;
- practiced one meaningful C++ architectural decision;
- documented the reasoning;
- produced at least one artifact that explains the work clearly.
