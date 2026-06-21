# Learning Path — the 12 stages, mapped

Agent Groundwork is organized as a **reference** (decide → build → prove). This page
re-frames the same material as a **learning progression** so you can climb it stage by
stage. It maps a common 12-stage agent-engineering ladder to where each topic lives
here — and is honest about the stages this playbook *points to* rather than teaches
(the hands-on coding of foundations/serving belongs in tutorials, not a decision
playbook).

> **Want to *do* each rung?** [`STARTERS.md`](./STARTERS.md) has a tiny hands-on
> starter (a few lines of code or a linked quickstart) for every stage below.

| Stage | Focus | Where in Groundwork | Coverage |
|---|---|---|---|
| **1 · Python + async foundations** | asyncio, FastAPI, event-driven, error handling, API integration | *prerequisite* — language-agnostic here; see FastAPI/asyncio docs | ⛳ prereq |
| **2 · LLM fundamentals** | context mgmt, model routing, token economics, latency, failure modes | [PLAYBOOK](./PLAYBOOK.md) #3 #7 · [ARCHITECTURE-REFERENCES](./ARCHITECTURE-REFERENCES.md) | ✅ |
| **3 · Tool calling + structured outputs** | typed schemas, Pydantic validation, error recovery, dynamic/MCP discovery | [PLAYBOOK](./PLAYBOOK.md) #1 · [OSS-LANDSCAPE](./OSS-LANDSCAPE.md) (Pydantic AI, MCP) | ✅ design |
| **4 · Memory + state** | short-term buffers, vector recall, compression, cross-session | [PLAYBOOK](./PLAYBOOK.md) #3 #4 · [OSS](./OSS-LANDSCAPE.md) (mem0/Letta/Zep) | ✅ |
| **5 · Single-agent workflows** | ReAct, plan-and-execute, self-reflection, iteration limits, graceful degradation | [PATTERNS](./PATTERNS.md) (agentic loop · evaluator-optimizer) · [PLAYBOOK](./PLAYBOOK.md) #2 #4 #5 | ✅ |
| **6 · Multi-agent orchestration** | supervisor patterns, message passing, conflict resolution, handoffs | [ARCHITECTURE-REFERENCES](./ARCHITECTURE-REFERENCES.md) · [AGENT-TYPES](./AGENT-TYPES.md) · [OSS](./OSS-LANDSCAPE.md) (LangGraph/CrewAI) | ✅ |
| **7 · Human-in-the-loop** | uncertainty detection, approval gates, audit trails, resume, intervention | [PLAYBOOK](./PLAYBOOK.md) #8 · [PATTERNS](./PATTERNS.md) · [GUARDRAILS](./GUARDRAILS-AND-SECURITY.md) | ✅ |
| **8 · Evaluation + QA** | eval harnesses, LLM-as-judge, regression, hallucination metrics | [BENCHMARKING](./BENCHMARKING.md) · [PLAYBOOK](./PLAYBOOK.md) #6 | ✅ |
| **9 · Observability + tracing** | distributed tracing, cost dashboards, latency, alerting | [OSS-LANDSCAPE](./OSS-LANDSCAPE.md) (Langfuse/Phoenix/MLflow/AgentOps) · [PLAYBOOK](./PLAYBOOK.md) #10 | ✅ |
| **10 · Security + guardrails** | prompt-injection, output filtering, PII, sandboxing, compliance | [GUARDRAILS-AND-SECURITY](./GUARDRAILS-AND-SECURITY.md) (+ OWASP) | ✅ |
| **11 · Production deployment** | serving (vLLM/SGLang), scaling, CI/CD, canary, rollback | [PRODUCTION](./PRODUCTION.md) | ✅ |
| **12 · Open source + portfolio** | ship publicly, architecture docs, demos, contribute | [README](./README.md) (open-sourcing) · [SKILL](./SKILL.md) · this repo as a worked example | 🟡 guidance |

## How to use the ladder
- **Beginners:** climb 1→12. Do the foundations (Stage 1) in a real tutorial, then use
  Groundwork for 2–11 as the *decision + reference* layer, building a small agent at
  each stage.
- **Already building:** you don't need the ladder — go straight to
  [DECIDE-FIRST](./DECIDE-FIRST.md) and pull the relevant chapters.
- **Honest scope:** Groundwork teaches the *decisions, patterns, and what-to-reuse* —
  not the line-by-line coding of async Python or a Kubernetes manifest. For those it
  points you at the right tools/docs. The thinking is the hard part; the syntax is
  look-up-able.

> The stages aren't strictly linear in practice — evals (8), observability (9), and
> guardrails (10) should be wired in from Stage 5, not bolted on at the end. Treat the
> ladder as a *curriculum*, and [CHECKLIST](./CHECKLIST.md) as the *non-negotiables*.
</content>
