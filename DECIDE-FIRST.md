# Decide First — the conversation before any code

> The single biggest lever is a 30-minute discussion *before* planning. Most rework
> comes from choosing the autonomy level and the orchestration shape implicitly, then
> discovering they were wrong. Make them explicit. Anthropic's guidance is blunt:
> **find the simplest thing that works; only add agency/complexity when simpler
> deterministic workflows fall short** ([Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)).

Answer these five before drawing an architecture.

## 1. What is the actual job? (and is an agent even warranted?)
- Write the task in one sentence and the **success criterion** you can measure.
- If the task is a fixed, known sequence → it's a **workflow** (LLM steps wired by
  code), not an agent. Cheaper, more reliable. Only reach for a **dynamic agent**
  (model directs its own tool use/control flow) when the path genuinely can't be
  pre-drawn. *Workflows vs agents is the first fork* (Anthropic).

## 2. How autonomous? (the autonomy slider)
Reliability is earned "one nine at a time" — design for *partial* autonomy with
verification, not full autonomy on day one.

| Level | Who decides / acts | Use when | Example |
|---|---|---|---|
| **Workflow** | Code decides path; LLM fills steps | Path is known; correctness matters | doc extraction, classify-then-route |
| **Human-in-the-loop (HITL)** | Agent proposes, human approves the risky move | Money/safety/irreversible actions; low tolerance for error | claim adjudication, refunds, code merges |
| **Supervised-autonomous** | Agent acts within bounds, human monitors + can stop | Bounded, reversible actions; volume needs it | triage, draft replies, enrichment |
| **Autonomous** | Agent acts end-to-end | Cheap-to-reverse, high-volume, well-evaled | log triage, retries, data cleanup |

**Decide the HITL boundary explicitly:** which actions *always* need a human, which
escalate on low confidence, which run free. Make "route to human"
(`needs_review`) a **first-class output**, not a failure. (12-Factor Agents:
["contact humans with tool calls"](https://github.com/humanlayer/12-factor-agents) — HITL is a factor, not an afterthought.)

## 3. Which orchestration shape? → see [PATTERNS.md](./PATTERNS.md)
Pick the *simplest* pattern that fits, and name it:
- **Single LLM + tools** → start here.
- **Prompt chaining / routing / parallelization** (deterministic workflows).
- **Orchestrator–workers / evaluator–optimizer** (dynamic, medium complexity).
- **DAG / state machine** when you need explicit branching, retries, and HITL gates
  (this is most production agents).
- **Closed-loop / self-healing / autonomous** only when warranted (and evaled).

## 4. Build vs. reuse? (default: reuse)
Anthropic: you can implement many patterns in **a few lines against the raw model
API** — and *if* you use a framework, understand its underlying code. So:
- **Reuse** an orchestration framework when you need state/branching/HITL/streaming
  at scale → see [OSS-LANDSCAPE.md](./OSS-LANDSCAPE.md) (LangGraph, OpenAI Agents
  SDK, CrewAI, smolagents, Pydantic AI, Claude Agent SDK…).
- **Reuse** for the hard sub-systems you should *never* hand-roll: **memory**
  (mem0/Letta/Zep), **evals + tracing** (MLflow/Langfuse/Phoenix/DeepEval),
  **durable execution / retries** (Temporal/Inngest/Restate), **HITL** (HumanLayer).
- **Build** only the thin domain glue (your tools, your schema, your policy).
- Rule of thumb: if a capability has a mature, benchmarked OSS option, **start by
  reading that repo**, not a blank file.

## 5. What will you measure? → see [PLAYBOOK.md](./PLAYBOOK.md) §metrics
Name the numbers *before* building: task success / eval score, cost per task,
p50/p95 latency, tool-call success rate, escalation (HITL) rate, and what "drift"
looks like. You can't improve what you don't measure.

---

## Fill-in spec (copy this into your repo's `AGENT_SPEC.md`)

```md
# Agent: <name>
Job (1 sentence): …
Success criterion (measurable): …

Workflow or dynamic agent?  [ workflow | dynamic ]   why: …
Autonomy level:             [ workflow | HITL | supervised-auto | autonomous ]
HITL boundary:
  - always human: …
  - escalate-on-low-confidence: …
  - runs free: …
Orchestration shape:        [ single+tools | chaining | routing | parallel |
                              orchestrator-workers | evaluator-optimizer |
                              DAG/state-machine | closed-loop | self-healing ]
Build vs reuse:
  - orchestration: [reuse <framework> | raw API]   why: …
  - memory:        [none | <lib>]
  - evals/obs:     [<tool>]
  - durable/retry: [in-proc | <engine>]
  - HITL channel:  [<tool/none>]
Tools (typed):              …
Output contract (schema):   …  (fail-closed default: …)
Containment (least-priv):   tool scopes …; never-exceed rule …; irreversible actions gated by …
Security / injection:       untrusted-data handling …; red-team suite [yes/no]
Metrics + targets:          success …, cost …, p95 latency …, HITL rate …, attack-success-rate …
Top 3 failure modes + guard: …
Eval set (size, how built): …   ⚠ note small-n / selection-on-test honestly
Benchmarks:                 task eval …; public suite for context [GAIA/SWE-bench/τ²-bench/BFCL/…]
```

> Containment & benchmarking aren't optional extras — see
> [GUARDRAILS-AND-SECURITY.md](./GUARDRAILS-AND-SECURITY.md) (keep the agent inside
> its rules) and [BENCHMARKING.md](./BENCHMARKING.md) (measure honestly; a leaderboard
> isn't your task).

> **Worked example — ClaimLens** (don't copy, contrast): *dynamic? no — a fixed
> **DAG**; autonomy = **HITL** (abstains to `needs_review` on low confidence);
> shape = tiered DAG (CV → perception → judge); reuse = raw provider APIs + a custom
> capability router (no heavy framework, by choice for a 24h build); metrics =
> claim-status accuracy (n=20, **directional** — selection-on-test, stated openly),
> cost/claim, p95 latency.* Its honest gap was **thin evals/guardrails** — exactly
> the thing this checklist forces you to name on day one.
</content>
