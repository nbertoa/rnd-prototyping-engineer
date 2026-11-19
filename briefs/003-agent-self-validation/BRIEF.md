# Brief 003 — Can BertaDevKit diagnostics let an agent validate its own editor changes?

| | |
|---|---|
| **Status** | Planned |
| **Engine** | UE 5.9 Preview (agent side) — BertaDevKit diagnostics already exist for 5.8 |
| **Size** | Medium (2 build weeks) |
| **Time spent** | — |
| **Video** | — |
| **Post** | — |
| **Reusable output** | BertaDevKit diagnostics callable by agents (MCP tool or console command contract) |

## 1. Question

If an agent can call BertaDevKit's existing audits and dumps (for example `Berta.UGC.Dump` and the UGC Editor Audit), does it catch and fix its own integration mistakes before reporting "done"?

## 2. Why it matters

The main weakness of agents in large editors is not writing changes but **knowing whether the change actually works**. In my own camera integration, the real bugs were wiring contracts (the default camera data outside the preset cycle, two spring arms competing), exactly what an audit can detect.

A validation layer for agents is rare and directly reusable in client prototyping. It also makes BertaDevKit relevant for the agentic era, not just for manual work.

## 3. Hypothesis

- With access to read-only diagnostics, the agent's defects-after-"done" drop noticeably on wiring tasks.
- Diagnostics need machine-readable output (structured text or JSON), not human-oriented logs, to be useful to agents.

## 4. Setup

- Pick 2–3 existing BertaDevKit diagnostics (UGC dump and audit first).
- Expose them to the agent: through Unreal MCP / Python toolsets if possible, otherwise as console commands with structured output.
- Seed known faults in a copy of the camera setup (default camera data outside `CameraPresets`, duplicate spring arm, missing manager class).

## 5. Method and metrics

Run the same repair task twice: agent **without** diagnostics vs agent **with** diagnostics.

Metrics: faults found, faults fixed correctly, false "done" reports, interventions, time (range).

## 6. Result

—

## 7. What worked / what did not

—

## 8. Verdict and recommendation

Expected outcome: decide whether "agent-callable diagnostics" becomes a standard BertaDevKit design rule (every new diagnostic ships with structured output).

## 9. Limits

Seeded faults are easier than real ones. A small number of runs; agent behaviour varies between runs.

## 10. Reusable output

- Structured-output contract for BertaDevKit diagnostics
- The MCP / console bridge, committed to BertaDevKit as an optional module

## 11. Applying it at work

Ship every new BertaDevKit diagnostic with structured output, so agents can check their own work on real projects.

## 12. Next question

Can the same diagnostics run as a reproducible test flow (setup → run → assert → report)? That links to the Experiment Runner idea in brief 004.

---

### Video idea

Same seeded bug, two runs side by side: the agent without diagnostics says "done" and the camera is broken; the agent with diagnostics runs the audit, reads the failure, fixes it, and the camera works.
