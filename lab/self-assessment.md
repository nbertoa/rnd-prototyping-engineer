# Skills Profile

## Positioning

I am developing toward an **R&D / Prototyping Engineer** profile centered on solving ambiguous technical problems, understanding requirements and constraints, estimating effort and tradeoffs, and turning ideas into working Unreal Engine prototypes.

My strength is not only building the prototype itself, but also being able to explain **how it works, why it was built that way, and what problem it is meant to solve**.

I use **AI as a constant engineering companion** throughout the workflow: research, exploration, implementation, debugging, documentation, comparison of alternatives, and communication.

A core engineering principle is **not reinventing the wheel unnecessarily**. Before building something from scratch, I want to evaluate whether Unreal already provides it, whether a mature plugin solves it well, whether adapting an existing solution is cheaper and safer, or whether custom development is justified.

## Core Strengths

- Unreal Engine prototyping
- C++ development
- Blueprint development
- Technical problem solving
- Requirements and context analysis
- Rapid experimentation and validation
- Gameplay systems
- Tooling and plugin integration
- Plugin evaluation and buy-vs-build decisions
- BertaDevKit development and reuse
- Technical research
- Communicating technical work clearly
- Creating technical presentations and demo videos
- AI-assisted engineering workflows

## Central Toolset: BertaDevKit

**BertaDevKit is my primary Unreal Engine R&D toolset.**

It is the main reusable layer I am building around my prototyping workflow. It gives me a place to capture solutions that repeatedly help with Unreal development instead of solving the same class of problem from scratch in every prototype.

It is intended to support both professional R&D work and personal projects, while keeping company-specific code, assets, confidential information, and intellectual property separate.

It currently spans reusable runtime utilities, editor tooling, diagnostics, hardware/software integration, and optional integrations with third-party gameplay and camera plugins.

The long-term goal is for BertaDevKit to become increasingly representative of how I work as an R&D engineer: fast to apply, explicit about tradeoffs, conservative where appropriate, useful for diagnosis and validation, reusable across prototypes, and comfortable integrating strong third-party solutions instead of duplicating them.

Repository: https://github.com/nbertoa/ue5-bertadevkit

## Current Level

### C++

**Self-assessment: 3.5 / 5**

Solid practical experience, with room to improve depth and consistency in architecture and larger-system design.

### Unreal Engine

Strong practical experience building and prototyping systems in Unreal Engine, with C++, Blueprints, BertaDevKit, existing plugins, and custom tooling.

## Areas to Improve

### C++ Architecture

- Better decomposition of systems
- Cleaner ownership and dependency boundaries
- Reusable Unreal-oriented architecture
- Maintainability as prototypes evolve
- Knowing when to keep a prototype simple and when to formalize it

### Tooling and Plugins

- Evolving BertaDevKit deliberately rather than only reactively
- Building reusable editor tools
- Developing clean Unreal Engine plugins
- Designing internal toolsets for faster prototyping
- Evaluating when to build, adapt, buy, or integrate an existing plugin
- Assessing plugin quality, maintenance, architecture fit, documentation, source access, licensing, and long-term risk
- Avoiding custom development when a proven solution already solves the problem well
- Extracting reusable infrastructure from successful prototypes

### GAS and Gameplay Systems

I already have meaningful experience here, but want to become more deliberate and advanced in:

- Gameplay Ability System architecture
- Ability and attribute design
- Gameplay effects and tags
- Reusable gameplay frameworks
- Translating design requirements into maintainable gameplay systems

### UI / CommonUI

- Stronger Unreal UI architecture
- CommonUI workflows
- Reusable UI systems
- Input-aware UI
- Better separation between presentation and gameplay logic

### VR / XR

- Building reliable Unreal prototypes for VR/XR
- Interaction patterns
- Device abstraction where useful
- Performance and usability constraints specific to immersive applications

### AI-Assisted R&D

AI is not a separate side skill. It is part of the engineering process.

I want to improve how I use it for:

- Rapid technical research
- Understanding unfamiliar APIs and systems
- Exploring implementation alternatives
- Generating and reviewing prototype code
- Debugging and hypothesis generation
- Architecture critique
- Documentation
- Creating presentation material
- Preparing scripts and narratives for demo videos
- Maintaining continuity across long-running R&D investigations
- Comparing third-party plugins and existing solutions before deciding to build
- Identifying repeated work that should become BertaDevKit tooling

The goal is not to outsource engineering judgment, but to use AI to **increase iteration speed, widen exploration, and reduce low-value manual work**.

## Supporting Knowledge

These are useful parts of my background, but they are not the primary direction of the profile:

- Computer graphics and rendering
- Performance analysis
- Systems-level programming
- Hardware and device integration when required by a project

## Communication

A completed prototype is not enough.

For important R&D work, I want to be able to communicate:

1. **What problem were we trying to solve?**
2. **What requirements and constraints mattered?**
3. **What approaches were considered?**
4. **What existing solutions or plugins were evaluated?**
5. **Why did we build, buy, adapt, or integrate?**
6. **What did I build?**
7. **How does it work?**
8. **Why was this approach chosen?**
9. **What did we learn?**
10. **What should happen next?**

Relevant outputs include:

- Prototype demos
- Technical notes
- Architecture explanations
- PowerPoint presentations
- Short technical videos
- Recommendations and next-step proposals

## Target Profile

A strong R&D / Prototyping Engineer who can take an unclear problem, understand its requirements, constraints, context, and expected timeline, and quickly turn it into a useful Unreal Engine prototype using the right combination of **C++, Blueprints, BertaDevKit, third-party plugins, and AI**.

The engineer should know when to build and when **not** to build, choosing existing solutions intelligently when they reduce risk, cost, and time without compromising the prototype's goals.

The engineer should also be able to explain the result clearly: **how it works, why the decisions were made, what it is useful for, what its limitations are, and what should happen next**.
