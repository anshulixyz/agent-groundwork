# Benchmarking & Evals — measure, but don't trust a leaderboard

Every agent needs benchmarking. But there are **two layers**, and conflating them is
a classic mistake:

1. **Public benchmarks** — standardized suites. Good for *directional* capability
   comparison and sanity. **Not** a substitute for your task.
2. **Your task eval** — a labelled set on *your* distribution, gated in CI. This is
   what actually tells you if the agent works for the job. **Decisive.**

## The 2026 standard suites (know them, report them, don't worship them)
| Benchmark | Measures | Notes |
|---|---|---|
| **GAIA** | general assistant: web + files + multi-doc reasoning | 466 Qs; top agents ~75% |
| **SWE-bench (Verified)** | real GitHub bug-fixes | the code-agent yardstick |
| **OSWorld** | computer-use on a real desktop | GUI/computer-use |
| **τ²-bench (tau2)** | tool-agent-**user** interactions + **policy adherence** | Sierra; dual-control (agent + simulated user); closest to customer-service/claims |
| **WebArena** | multi-step browser tasks | web navigation |
| **Terminal-Bench** | terminal/CLI tasks | ops/devtools |
| **BFCL** (Berkeley Function-Calling Leaderboard) | tool/function-calling correctness | most relevant to tool design |
| **METR HCAST / time-horizons** | longest task an agent finishes 50% of the time | capability trend, not pass/fail |
| **AgentBench** | multi-environment agent tasks | breadth |

Sources: [AI Agent Benchmarks 2026](https://benchmarkingagents.com/agent-benchmarks/) ·
[the 6 that matter](https://decodethefuture.org/en/ai-agent-benchmarks-2026/) ·
[multi-dimensional enterprise eval framework (arXiv)](https://arxiv.org/pdf/2511.14136).

## ⚠ Why a leaderboard number is not the answer
- **Inflation 5–15 pts** from data **contamination**, **scaffolding** differences,
  and **single-run** reporting.
- **Reward hacking is real:** in Apr 2026, UC Berkeley researchers showed an automated
  agent **broke all eight major agent benchmarks** — reaching near-perfect scores
  **without solving a single task** ([source](https://benchmarkingagents.com/agent-benchmarks/)).
- **Benchmarks ≠ your distribution.** A 75% on GAIA says nothing about your refund
  policy or your claim photos.

**So:** use public suites to pick a base model / framework directionally; **decide
on a task eval you control.**

## How to benchmark *your* agent (the methodology that matters)
- **Labelled eval set from day one** — representative of production; grow it.
- **Multi-run** (temperature/order variance) — report the spread, not one lucky run.
- **Hold-out / k-fold** — don't select your "best" config on the same rows you report
  (selection-on-test inflates the number).
- **Regression-gate in CI** — every change re-runs the eval; a drop fails the build.
- **Calibration** — if you emit confidence, measure **ECE**; calibrated confidence is
  what makes "abstain / escalate" defensible.
- **Multi-dimensional, not just accuracy** — also **cost/task, p50/p95 latency,
  policy-adherence, safety/attack-success-rate (see [GUARDRAILS-AND-SECURITY.md](./GUARDRAILS-AND-SECURITY.md)),
  and HITL/override rate.** τ²-bench-style *policy adherence* matters for regulated work.
- **LLM-as-judge** is useful but biased — anchor it with a rubric, spot-check against
  humans, and never let the judge model also be the system under test.
- **Trace + replay** every run for error analysis (MLflow/Langfuse/Phoenix/AgentOps).

## Are you actually doing this? (the planning check)
Tick these in [`DECIDE-FIRST.md`](./DECIDE-FIRST.md) / [`CHECKLIST.md`](./CHECKLIST.md):
- [ ] A task eval set exists, with its **size + how it was built** written down.
- [ ] Reported with **variance** and **honest about small-n / selection-on-test**.
- [ ] **Regression-gated in CI**; multi-dimensional (accuracy + cost + latency + safety).
- [ ] A **red-team suite** for injection/abuse (security is part of the benchmark).
- [ ] Confidence **calibrated** if you abstain/escalate on it.

## Worked example — ClaimLens (the anti-pattern to outgrow, honestly)
- Multi-strategy eval + cost/latency tracked + a confusion matrix + a dashboard ✅.
- **But:** n=20 labelled rows and the harness **selected the best config on those same
  rows** (selection-on-test) → the headline (0.60→0.80 depending on judge) is
  **directional, not a true accuracy**. The honest fix is exactly the methodology
  above: a bigger held-out set, k-fold, calibration, and a red-team suite. *Reporting
  the number as directional — and saying why — is the right move when you can't yet
  report a robust one.*
</content>
