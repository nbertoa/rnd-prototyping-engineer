# R&D / Prototyping Engineer Roadmap

## Direction

The goal is to strengthen a professional profile built around solving ambiguous technical problems, understanding requirements and constraints, prototyping effectively in Unreal Engine, and communicating the result clearly.

**BertaDevKit is the central reusable toolset supporting that process.**

The roadmap is intentionally evidence-driven: each area should produce something observable — a tool, plugin improvement, prototype, case study, technical note, presentation, or demo video.

---

## Q4 2026 — Consolidate the Core

### 1. BertaDevKit as the main R&D platform

Focus on making BertaDevKit increasingly useful as the default starting point for Unreal R&D work.

Priorities:

- Identify repeated friction from current and recent prototypes
- Turn valuable repeated solutions into reusable tooling
- Improve consistency between plugins
- Keep integrations modular and optional
- Improve diagnostics, validation, and observability where they reduce investigation time
- Keep documentation aligned with actual behavior

Evidence:

- At least one meaningful BertaDevKit improvement derived from a real R&D need
- A short write-up explaining the problem, solution, tradeoffs, and reuse value

### 2. C++ architecture

Use real BertaDevKit and prototype work as the learning surface.

Priorities:

- Ownership and lifetime
- Module and plugin boundaries
- Interfaces and dependency direction
- Composition over unnecessary inheritance
- Data-driven design where appropriate
- Knowing when prototype code should remain simple and when it should be formalized

Evidence:

- One architecture review of an existing subsystem or plugin
- One documented refactor with a clear reason for the change

---

## Q1 2027 — Gameplay Systems

### GAS and gameplay architecture

Deepen practical knowledge rather than learning GAS in isolation.

Priorities:

- Ability architecture
- Attributes, effects, and tags
- Input integration
- Diagnostics and readiness checks
- Reusable gameplay patterns
- Understanding where GAS is useful and where it is unnecessary

Use BertaDevKit and its optional integrations where they genuinely improve investigation or prototyping speed.

Evidence:

- One focused gameplay prototype
- One technical explanation covering requirements, architecture, tradeoffs, and result

---

## Q2 2027 — UI / CommonUI and Prototype Completeness

### UI / CommonUI

Improve the ability to build complete prototypes instead of leaving UI as an afterthought.

Priorities:

- CommonUI mental model and architecture
- Input-aware UI
- Reusable screen and widget structure
- C++ / Blueprint responsibility boundaries
- Separation of presentation and gameplay state

Evidence:

- One prototype with a deliberate UI architecture
- Reusable patterns or utilities promoted into BertaDevKit only if they solve a recurring need

---

## Q3 2027 — VR / XR and Broader R&D Capability

VR/XR is a secondary specialization, not the defining identity of the profile.

Priorities:

- Unreal XR architecture
- Interaction patterns
- Device-specific constraints
- Performance and usability considerations
- Fast evaluation of unfamiliar XR requirements

Evidence:

- One small XR investigation or prototype
- A concise recommendation explaining feasibility, constraints, findings, and next steps

---

## Continuous Track — AI-Assisted R&D

AI should accompany the whole roadmap rather than live in a separate learning phase.

Use it deliberately for:

- Technical research
- API exploration
- Hypothesis generation
- Implementation alternatives
- Code review and architecture critique
- Debugging support
- Documentation
- Requirement decomposition
- Estimation support
- Presentation structure
- Demo-video scripts
- Maintaining continuity across long investigations

The important skill is not prompting by itself. It is learning how to combine AI speed with engineering judgment, verification, context, and clear decision-making.

---

## Continuous Track — Communication

For significant R&D work, produce communication artifacts when useful:

- What was requested?
- What context and constraints mattered?
- What options were considered?
- What was prototyped?
- How does it work?
- Why was that approach chosen?
- What worked and what did not?
- What are the limitations?
- What should happen next?

Preferred outputs:

- Technical notes
- Short case studies
- PowerPoint presentations
- Demo videos

---

## Guiding Rule

Do not optimize for collecting technologies, courses, or isolated exercises.

Optimize for becoming better at this loop:

**Understand → Research → Prototype → Validate → Tool → Explain → Reuse**

When a solution proves reusable, consider whether it belongs in **BertaDevKit**.
