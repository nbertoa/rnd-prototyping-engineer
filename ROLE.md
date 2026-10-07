# What an R&D / Prototyping Engineer Does

## Definition

An **R&D / Prototyping Engineer** takes technical uncertainty and turns it into evidence that helps a team make better decisions.

The role is not defined by inventing something that has never existed before. R&D can also mean investigating an unfamiliar technology, validating whether an idea is feasible, comparing implementation paths, identifying risks, integrating existing solutions, and building focused prototypes that answer concrete questions.

A strong R&D / Prototyping Engineer should be able to:

- understand an ambiguous problem before coding;
- clarify requirements, context, constraints, risks, and expected timelines;
- identify the key unknowns that actually need investigation;
- research technologies, APIs, plugins, tools, and prior art quickly;
- decide what should be built, bought, integrated, extended, or discarded;
- build the smallest useful prototype that can validate the important assumptions;
- measure or observe enough evidence to determine what worked and what did not;
- explain tradeoffs, limitations, risks, and recommended next steps;
- turn repeated solutions into reusable tooling when that creates real value;
- communicate the result clearly through documentation, presentations, demos, and videos.

## What R&D Is Not

R&D is not automatically:

- using a new technology because it is new;
- building everything from scratch;
- producing large prototypes when a small spike would answer the question;
- accumulating courses, frameworks, or certifications;
- turning every experiment into production code;
- inventing novelty for its own sake.

The useful output of R&D is **reduced uncertainty**.

A prototype succeeds when it produces enough evidence to make a better decision, even if the conclusion is that an approach should not be pursued.

## Working Loop

The default loop for this profile is:

**Understand → Research → Prototype → Validate → Tool → Explain → Reuse**

### Understand

Clarify the real problem, stakeholders, requirements, constraints, timeline, and definition of success.

### Research

Investigate Unreal-native solutions, third-party plugins, APIs, existing implementations, technical risks, and alternative approaches.

### Prototype

Build the smallest implementation that answers the important technical questions.

### Validate

Test the assumptions. Gather evidence. Identify failure modes, limitations, integration costs, and remaining unknowns.

### Tool

If a solution or diagnostic pattern repeatedly creates value, consider promoting the reusable portion into tooling such as **BertaDevKit**.

### Explain

Communicate what was attempted, how it works, why decisions were made, what was learned, and what should happen next.

### Reuse

Carry useful knowledge, tools, patterns, integrations, and decision criteria into future work.

## Build vs. Buy vs. Integrate

A core R&D skill is knowing when **not** to build.

Before implementing a custom solution, evaluate:

1. Does Unreal Engine already solve the problem well enough?
2. Is there a mature plugin, library, or tool that solves it?
3. Is buying it cheaper than the engineering and maintenance cost of rebuilding it?
4. Can an existing solution be integrated or extended safely?
5. Only then: is custom implementation justified?

A successful investigation may conclude that purchasing and integrating a strong plugin is the best engineering decision.

## Professional Work and Public Evidence

Most client R&D work cannot be published directly because it may contain proprietary code, confidential requirements, private assets, or internal technical decisions.

That does not prevent building a public R&D profile.

The public evidence for this profile should come from:

- generic tools and reusable infrastructure such as **BertaDevKit**;
- independent technical investigations;
- personal prototypes and experiments;
- plugin evaluations and integration studies;
- public case studies that contain no confidential information;
- architecture notes, presentations, and demo videos;
- personal projects used as R&D laboratories.

Client work can develop the skill; public personal work can demonstrate the skill.

## Personal R&D Laboratory

Personal projects provide a place to investigate freely and create publishable evidence.

They do not need to be pure technology demos. A real product or creative experience can become an R&D laboratory when it contains meaningful technical uncertainty that is deliberately investigated.

For example, an interactive or audiovisual biblical experience can be used to investigate:

- rapid interactive-narrative prototyping;
- cinematic camera systems;
- dialogue and branching frameworks;
- inspection and discovery systems;
- CommonUI and input architecture;
- save/load and persistence;
- NPC and gameplay-state orchestration;
- third-party plugin interoperability;
- reusable authoring tools;
- AI-assisted content and engineering workflows;
- Steam packaging and productization.

The creative product and the R&D investigation are related but distinct: **storytelling is the domain; R&D is the method used to reduce technical uncertainty and improve how the experience is built.**

## BertaDevKit

**BertaDevKit is the primary reusable technical infrastructure for this profile.**

It may be used in both professional and personal projects when appropriate, while keeping proprietary client code, assets, requirements, and confidential information separate.

BertaDevKit should grow from repeated real needs, not from speculative feature accumulation.

A capability belongs in BertaDevKit when it has clear reuse value across projects, reduces repeated engineering effort, improves diagnosis or validation, or creates a reliable integration workflow.

## AI as an R&D Companion

AI is part of the day-to-day engineering workflow rather than a separate specialization.

It can accelerate:

- technical research;
- API discovery;
- hypothesis generation;
- implementation exploration;
- code and architecture review;
- debugging;
- plugin comparison;
- requirement decomposition;
- documentation;
- estimation support;
- presentation and demo preparation;
- continuity across longer investigations.

The engineer remains responsible for judgment, verification, technical understanding, and the final decision.

## Evidence of Growth

Progress in this profile should be demonstrated primarily through evidence such as:

- prototypes that answered meaningful questions;
- technical investigations with clear conclusions;
- plugin buy/integrate/build decisions;
- reusable BertaDevKit improvements;
- architecture reviews and justified refactors;
- case studies;
- technical documentation;
- presentations;
- demo videos;
- completed personal experiments or products.

The objective is to become increasingly effective at taking an unclear technical problem and producing **clarity, evidence, a working prototype, and a well-reasoned recommendation**.
