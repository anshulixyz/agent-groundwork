# Pre-flight Checklist

One page. Run before planning, and again before you ship. Don't tick a box you can't
back with a sentence (or a number).

## Decide first ([DECIDE-FIRST.md](./DECIDE-FIRST.md))
- [ ] Job in one sentence + a **measurable** success criterion.
- [ ] Decided: **workflow** (code-orchestrated) or **dynamic agent** — and why.
- [ ] **Autonomy level** chosen: workflow / HITL / supervised-auto / autonomous.
- [ ] **HITL boundary** named: what always needs a human, what escalates, what runs free.
- [ ] **Orchestration shape** named ([PATTERNS.md](./PATTERNS.md)) — simplest that fits.
- [ ] **Build-vs-reuse** decided per sub-system ([OSS-LANDSCAPE.md](./OSS-LANDSCAPE.md)); read the repos you're reusing.
- [ ] `AGENT_SPEC.md` filled in.

## Build ([PLAYBOOK.md](./PLAYBOOK.md))
- [ ] **Tools**: few, typed, single-job; outputs validated; tool output treated as untrusted.
- [ ] **Control flow** explicit where possible; loops are capped; steps idempotent + resumable.
- [ ] **Context** curated (distilled, trust-tagged); durable facts persisted outside the window.
- [ ] **Output contract**: schema-validated, **fails closed** to the safe outcome.
- [ ] **Self-healing**: retry+backoff, fallback chain + circuit breaker, every heal logged.
- [ ] **HITL**: "escalate to human" is a first-class output with confidence + rationale.
- [ ] **Guardrails & containment** ([GUARDRAILS-AND-SECURITY.md](./GUARDRAILS-AND-SECURITY.md)): untrusted-data discipline (external text ≠ instructions), least-privilege tools, **no action outside declared scope**, human gate on irreversible actions.
- [ ] **Secrets**: from env / gitignored `.env`; never logged; not in any shipped artifact.

## Prove it
- [ ] **Eval set exists** (state its size + how built; flag small-n / selection-on-test honestly).
- [ ] **Benchmarked** ([BENCHMARKING.md](./BENCHMARKING.md)): task eval + a relevant public suite for context; multi-run + multi-dimensional (accuracy · cost · latency · safety).
- [ ] **Red-team suite** for prompt-injection / tool-abuse, gated in CI; attack-success-rate tracked.
- [ ] **Tested at the real boundary** (HTTP/tool I/O), not just internal functions.
- [ ] **CI gates**: lint + tests + output/contract check + (if docs exist) docs-sync + UI parse.
- [ ] **Metrics captured**: success/eval, cost/task, p50/p95 latency, tool-success, HITL rate.
- [ ] **Cost is a known dial** (cheap vs accurate config), not an accident.
- [ ] **Observability**: per-run trace you (or an auditor) can read.

## Ship & maintain
- [ ] **Honest limits** written down (a prioritized backlog of what's thin).
- [ ] **Commits scoped** (no unrelated changes bundled in).
- [ ] **Top-3 failure modes** named, each with a guard.
- [ ] Decision trail / changelog kept (it's also how you write the next playbook).

> If most failures are orchestration failures, most of these boxes are about
> orchestration — which is the point.
</content>
