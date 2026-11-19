# Nicolás Bertoa — R&D / Prototyping Engineer (Unreal Engine)

I turn ambiguous technical questions into **working prototypes, measured evidence and clear recommendations**, in Unreal Engine (C++ / Blueprints), using AI agents as a daily engineering tool.

This repository is my public R&D log. Every entry is a short **R&D Brief**: one question, one small prototype, one measured answer, one short video and one reusable artifact I keep using afterwards.

- Portfolio and write-ups: [nbertoa.com](https://nbertoa.com)
- Main reusable toolset: [BertaDevKit](https://github.com/nbertoa/ue5-bertadevkit)
- How I work: [METHOD.md](METHOD.md)

---

## Current focus: agentic Unreal prototyping, with evidence

Unreal Engine is moving fast towards AI-assisted development (Unreal MCP and Unreal Assistant in UE 5.9, AI as a pillar of UE6). There is a lot of hype and very little measured evidence about what agents actually do well inside a real Unreal project.

My briefs answer concrete questions like:

- Which Unreal tasks can an agent complete from a written spec, and how many interventions does it need?
- How can an agent **verify its own work** in the editor instead of just claiming success?
- When does it still make sense to buy a plugin, and when is it cheaper to have an agent build it?
- Which orchestration approach (StateTree, Logic Driver, plain Blueprints) stays maintainable when requirements change?

## R&D Briefs

| # | Question | Status | Video | Post |
|---|----------|--------|-------|------|
| [001](briefs/001-ue59-mcp-field-report/BRIEF.md) | What can an agent rebuild from a spec in UE 5.9 with Unreal MCP? | Planned | — | — |
| [002](briefs/002-plugins-in-the-ai-era/BRIEF.md) | Buy, build or let an agent build it: how should plugin decisions change? | Planned | — | — |
| [003](briefs/003-agent-self-validation/BRIEF.md) | Can BertaDevKit diagnostics let an agent validate its own editor changes? | Planned | — | — |
| [004](briefs/004-experience-orchestration/BRIEF.md) | StateTree vs Logic Driver vs agent-generated graph for cross-plugin orchestration | Planned | — | — |

Full index and status rules: [briefs/README.md](briefs/README.md).

## Repository map

| Folder | What it is | Audience |
|--------|------------|----------|
| [`briefs/`](briefs/) | Finished and planned R&D Briefs | Public, curated |
| [`playbooks/`](playbooks/) | How a brief is run, recorded and published | Public |
| [`toolkit/`](toolkit/) | The Unreal stack I prototype with and how I decide what to buy | Public |
| [`METHOD.md`](METHOD.md) | My R&D method: understand → research → prototype → validate → tool → explain → reuse | Public |
| [`ROADMAP.md`](ROADMAP.md) | What I am investigating next and why | Public |
| [`lab/`](lab/) | Raw working notebook: integration logs, plugin manuals, drafts | Working notes, not curated |

## Ground rules

- **Evidence over claims.** Results say what was measured, how, and with which limits. Uncertain results are given as ranges.
- **Small and finished beats big and pending.** A brief is sized to fit in one or two weeks of spare time.
- **Every brief leaves something reusable**: a BertaDevKit feature, a template, a prompt spec, a decision rule.
- **No client material.** Client work stays private. Everything here comes from personal prototypes, generic tooling and public technology.
