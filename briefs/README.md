# R&D Briefs

A brief is the unit of work of this repository: **one question → one small prototype → one measured answer → one video → one reusable artifact.**

Template: [_TEMPLATE.md](_TEMPLATE.md). How a brief is run: [../playbooks/iteration-cycle.md](../playbooks/iteration-cycle.md).

## Index

| # | Question | Status | Started | Published |
|---|----------|--------|---------|-----------|
| [001](001-ue59-mcp-field-report/BRIEF.md) | What can an agent rebuild from a spec in UE 5.9 with Unreal MCP? | Planned | — | — |
| [002](002-plugins-in-the-ai-era/BRIEF.md) | Buy, build or let an agent build it: how should plugin decisions change? | Planned | — | — |
| [003](003-agent-self-validation/BRIEF.md) | Can BertaDevKit diagnostics let an agent validate its own editor changes? | Planned | — | — |
| [004](004-experience-orchestration/BRIEF.md) | StateTree vs Logic Driver vs agent-generated graph for cross-plugin orchestration | Planned | — | — |

## Status values

| Status | Meaning |
|--------|---------|
| **Planned** | Question and scope written, nothing built yet |
| **Active** | Currently being prototyped (keep at most one active at a time) |
| **Recording** | Result reached, video and post being produced |
| **Published** | Brief, video and post are public |
| **Parked** | Stopped on purpose; the reason is written in the brief |

A parked brief is a valid outcome. "We tried it and it is not worth it" is real R&D evidence, as long as the reason is written down.

## Folder layout per brief

```text
briefs/NNN-short-slug/
├── BRIEF.md        ← the public write-up (from the template)
├── media/          ← thumbnails, screenshots, GIFs (keep video files out of git; link to YouTube)
└── assets/         ← specs, prompts, data tables, small scripts used in the experiment
```

## Choosing the next brief

Pick the candidate that scores best on these four questions (1–3 each):

1. **Useful to me:** will the result change how I work on real projects?
2. **Visible:** can the result be shown in a 60–90 second video?
3. **Differentiating:** is good, measured evidence on this rare right now?
4. **Fits:** can it be finished in 1–2 weeks of spare time with tools I already own?

Write candidate ideas at the bottom of [../ROADMAP.md](../ROADMAP.md), not as new folders, until one is picked.
