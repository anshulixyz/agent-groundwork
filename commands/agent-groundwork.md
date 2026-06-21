---
description: Build/design an agent the right way — runs the Agent Groundwork playbook (decide-first → patterns → guardrails → eval → ship).
argument-hint: [what you're building]
---

Use the **Agent Groundwork** playbook (in `~/.claude/skills/agent-groundwork/`, or this
repo's `.claude/skills/agent-groundwork/`) to help me build an agent.

Drive it in order — do NOT jump to code:

1. **Decide first** (`DECIDE-FIRST.md`): ask me the pre-build questions before proposing
   any architecture —
   - workflow (code-orchestrated) vs dynamic agent?
   - autonomy level + the HITL boundary (what always needs a human / escalates / runs free)?
   - orchestration pattern (`PATTERNS.md`; `AGENT-TYPES.md` for the kind of agent)?
   - build-vs-reuse per sub-system (`OSS-LANDSCAPE.md`)?
2. Reference a proven shape from `ARCHITECTURE-REFERENCES.md`.
3. Build against the 8 competencies (`PLAYBOOK.md`); keep it inside its rules
   (`GUARDRAILS-AND-SECURITY.md`).
4. Plan evals (`BENCHMARKING.md`) and, if shipping, production (`PRODUCTION.md`).
5. Produce a filled `AGENT_SPEC.md` (template in `DECIDE-FIRST.md`). Be honest about
   evals/limits — directional is fine if you say so.

New to agents? Offer the `LEARNING-PATH.md` ladder + `STARTERS.md` stubs instead.

What I'm building: $ARGUMENTS
</content>
