# Roadmap

The roadmap is a sequence of **briefs**, not a list of topics to study. Each skill area (GAS, UI, XR…) appears only as part of a brief that answers a concrete question.

Previous roadmap (October 2026): [lab/archive/roadmap-2026-10.md](lab/archive/roadmap-2026-10.md).

## Main thread: agentic Unreal prototyping, with evidence

**Why this thread:** UE 5.9 Preview ships Unreal MCP, Unreal Assistant and expanded agent toolsets, and UE6 (early access targeted around the end of 2027) is planned around AI integration. Engineers who can show **measured** results about agents in real Unreal projects are rare. This thread also makes my daily work faster, which is the point.

Supporting skills (GAS, CommonUI, cameras, orchestration, XR) are used as the test material for this thread rather than as separate study goals.

## Next briefs (in order)

| Order | Brief | Why this order |
|-------|-------|----------------|
| 1 | [002 — Plugins in the AI era](briefs/002-plugins-in-the-ai-era/BRIEF.md) | Shortest brief, mostly analysis: a quick first publication that gets the cycle going |
| 2 | [001 — UE 5.9 MCP field report](briefs/001-ue59-mcp-field-report/BRIEF.md) | Timely while 5.9 is new; reuses tasks already solved by hand |
| 3 | [003 — Agent self-validation](briefs/003-agent-self-validation/BRIEF.md) | Builds on 001's failure modes; strengthens BertaDevKit |
| 4 | [004 — Experience orchestration](briefs/004-experience-orchestration/BRIEF.md) | Closes the Logic Driver question and produces the Sanctuary vertical slice |

## Calendar (alternating build / write weeks)

Small brief = 1 build week + 1 write week (2 weeks). Medium brief = 2 build weeks + 2 write weeks (4 weeks).

| Period | Brief | Evidence |
|--------|-------|----------|
| Dec 2026 | 002 — Plugins in the AI era (small) | First post + video; updated purchase rule |
| Jan 2027 | 001 — UE 5.9 MCP field report (medium) | Delegation table + spec templates |
| Feb 2027 | 003 — Agent self-validation (medium) | Agent-callable BertaDevKit diagnostics |
| Mar 2027 | 004 — Experience orchestration (medium) | Orchestration decision + Sanctuary vertical slice |
| Q2 2027 | 3–4 briefs from the candidates list | Picked with the scoring in `briefs/README.md` |
| Q3 2027 | Consolidated write-up: "A year of agentic Unreal prototyping" | Long-form post + talk-ready deck |

If UE 5.9 ships its full release during this period, re-run the key tasks of brief 001 on it and publish a short follow-up.

## Candidate briefs (not started)

Score each 1–3 on useful / visible / differentiating / fits before picking (see [briefs/README.md](briefs/README.md)).

- **GAS feature from spec, end to end:** agent builds a new ability with cost, cooldown, tags and HUD; compare with the hand-built Jump / Sprint.
- **Experiment Runner:** reproducible setup → run → measure → report flows for performance and integration tests.
- **Runtime voice NPC, fully local:** speech-to-text → LLM → text-to-speech with runtime audio import; measure latency per stage.
- **CommonUI from spec:** can an agent build an input-aware menu with gamepad navigation?
- **Plugin integration runbook generator:** turn vendor manuals plus my integration logs into a reproducible setup checklist an agent can follow.
- **XR interaction spike:** grab and inspect in VR with native tools vs a plugin, only if an XR need appears.
- **UE6 readiness:** what parts of a UE5 C++ / Blueprint prototype are likely to port cleanly to Verse and Scene Graph, based on the public UE6 branch.

## Rules

- One active brief at a time.
- A sale or a new feature is not a reason to start a brief; a question is.
- Each quarter, review: which briefs changed how I work? Do more of those.
