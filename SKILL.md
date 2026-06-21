---
name: agent-groundwork
description: >-
  Decision-first, vendor-neutral playbook for building LLM agents. Use when
  starting a new agent/agentic feature, scoping a hackathon agent build, choosing
  an autonomy level (workflow / human-in-the-loop / autonomous) or orchestration
  pattern (DAG, state machine, closed-loop, self-healing), deciding which agent
  framework/library to reuse vs build, or setting up agent evals, reliability,
  cost/latency, and human-in-the-loop. Triggers on "build an agent", "design an
  agent", "agentic workflow", "which agent framework", "agent evals", "how
  autonomous should this be".
---

# Agent Groundwork (skill)

A kickstart for building reliable agents. Core thesis: **most agent failures are
orchestration failures, not model failures** — so spend your effort on the harness.

## Install (one command)
```bash
git clone https://github.com/anshulixyz/agent-groundwork ~/.claude/skills/agent-groundwork
```
Project-scoped instead? Clone into `<your-repo>/.claude/skills/agent-groundwork` to
share it with collaborators via git.

## Invoke
- **Model-invoked (default):** once installed, Claude auto-loads this skill when you
  ask agent-building things — *"help me design an agent", "which agent framework?",
  "how autonomous should this be?"* (it triggers on the `description` above). Just
  describe what you're building.
- **Explicit:** say *"use the agent-groundwork skill"*.
- **As a literal slash command (optional):** copy the bundled wrapper —
  `cp ~/.claude/skills/agent-groundwork/commands/agent-groundwork.md ~/.claude/commands/`
  — then type **`/agent-groundwork`** in Claude Code. (Skills are model-invoked; a
  slash command is a separate Claude Code feature, so it's an opt-in wrapper.)

## How to drive this skill

When the user is about to build an agent (or asks how), do this in order — **don't
skip straight to code**:

1. **Decide first.** Walk the user through [`DECIDE-FIRST.md`](./DECIDE-FIRST.md):
   - Is this a **workflow** (code-orchestrated) or a **dynamic agent**?
   - **Autonomy level** — workflow / human-in-the-loop / supervised-autonomous /
     autonomous? Name the **HITL boundary** explicitly.
   - **Orchestration shape** from [`PATTERNS.md`](./PATTERNS.md) — pick the simplest
     that fits.
   - **Build vs reuse** per sub-system; consult [`OSS-LANDSCAPE.md`](./OSS-LANDSCAPE.md)
     and prefer reading a mature repo over a blank file.
   - Produce a filled `AGENT_SPEC.md` (template at the bottom of `DECIDE-FIRST.md`).
   **Ask the clarifying questions before proposing an architecture.**

2. **Build** against [`PLAYBOOK.md`](./PLAYBOOK.md) — the 8 competencies, each with
   practices, what to **measure**, and what to **reuse**.

3. **Make it safe** with [`GUARDRAILS-AND-SECURITY.md`](./GUARDRAILS-AND-SECURITY.md):
   treat all external text as untrusted data (prompt-injection is OWASP #1), apply
   defense-in-depth, and **keep the agent inside its rules** — least-privilege tools,
   no action outside scope, human gate on irreversible actions.

4. **Prove it** with [`BENCHMARKING.md`](./BENCHMARKING.md) + [`CHECKLIST.md`](./CHECKLIST.md):
   a task eval from day one (honest about small-n; a public suite only for context),
   multi-run + multi-dimensional (accuracy · cost · latency · safety), a red-team
   suite, **test at the real boundary** (HTTP/tool I/O, not internals), CI gates,
   fail-closed outputs, and a written list of honest limits.

## Rules this skill enforces
- **Simplest thing that works first** — add agency/complexity only when deterministic
  workflows fall short (Anthropic).
- **HITL is a design choice, decided up front** — and "escalate to human" is a
  first-class output, never a failure.
- **Reuse the hard sub-systems** (memory, evals/tracing, durable execution, HITL);
  build only the thin domain glue.
- **Honesty over headline numbers** — report variance, small-n, and what's thin.
- **No secrets in artifacts**; **scoped commits**; **keep a decision trail**.

## Files
`README.md` (overview) · `LEARNING-PATH.md` (12-stage ladder) · `STARTERS.md`
(hands-on stubs per stage) · `DECIDE-FIRST.md` (start here) · `AGENT-TYPES.md` ·
`PATTERNS.md` · `ARCHITECTURE-REFERENCES.md` ·
`PLAYBOOK.md` · `GUARDRAILS-AND-SECURITY.md` · `BENCHMARKING.md` · `PRODUCTION.md` ·
`OSS-LANDSCAPE.md` · `CHECKLIST.md`.

> Sources are cited inline in each doc (Anthropic Building Effective Agents,
> 12-Factor Agents, framework/eval/memory comparisons). Replace recommendations with
> your own measured numbers as you get them.
</content>
