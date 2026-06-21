# Hands-on Starters — get your hands on each rung

One tiny starter per [learning-path](./LEARNING-PATH.md) stage. These are **minimal,
illustrative sketches** to do *now* — stdlib/obvious where the code is real, and
labeled **`# sketch`** where a framework's exact API may drift (follow the linked
quickstart for the current signature). The goal is momentum, not copy-paste production
code.

> Honesty: the thinking (decide-first, patterns, guardrails) is the hard part and lives
> in the other docs. These stubs just get you moving.

### Stage 1 — Python + async foundations
```python
# pip install fastapi uvicorn httpx   →   uvicorn main:app --reload
import httpx
from fastapi import FastAPI
app = FastAPI()

@app.get("/agent")
async def run(q: str):
    async with httpx.AsyncClient() as c:        # async I/O — never block the event loop
        r = await c.get("https://httpbin.org/get", params={"q": q})
    return {"echo": r.json()["args"]}
```
Real: [FastAPI](https://fastapi.tiangolo.com) · [asyncio](https://docs.python.org/3/library/asyncio.html)

### Stage 2 — LLM fundamentals (call + fallback + cost)
```python
# sketch — pattern over any provider SDK
def complete(model, messages): ...            # returns (text, usage)

def judge(messages):
    for model in ["strong-model", "cheap-model"]:     # best-first routing
        try:
            text, usage = complete(model, messages)
            cost = usage.input/1e6*PIN + usage.output/1e6*POUT   # token economics
            return text
        except (RateLimit, OutOfCredit):              # failure mode → fall back
            continue
    return mock(messages)                              # always return something valid
```
Real: [Anthropic](https://docs.claude.com) / [OpenAI](https://platform.openai.com/docs) quickstart

### Stage 3 — Tool calling + structured outputs
```python
# pip install pydantic
from typing import Literal
from pydantic import BaseModel, ValidationError

class Verdict(BaseModel):
    status: Literal["supported", "contradicted", "needs_info"]
    severity: Literal["none", "low", "medium", "high"]

try:
    v = Verdict.model_validate_json(model_json_output())   # untrusted output → validate
except ValidationError:
    v = Verdict(status="needs_info", severity="none")      # fail closed
```
Real: [Pydantic](https://docs.pydantic.dev) · tool-use: [Anthropic](https://docs.claude.com/en/docs/agents-and-tools/tool-use/overview) · discovery: [MCP](https://modelcontextprotocol.io)

### Stage 4 — Memory + state
```python
history = []                                   # short-term buffer
def remember(msg):
    history.append(msg); del history[:-20]     # keep last 20 (a crude context window)

# long-term recall — sketch (mem0):
# from mem0 import Memory; m = Memory()
# m.add("user prefers email", user_id="u1");  m.search("contact?", user_id="u1")
```
Real: [mem0](https://github.com/mem0ai/mem0) · [Letta](https://github.com/letta-ai/letta) · [Zep](https://github.com/getzep/zep)

### Stage 5 — Single-agent workflow (the agentic loop)
```python
def agent(goal, tools, max_steps=6):           # CAP the loop — no infinite agents
    ctx = [goal]
    for _ in range(max_steps):
        step = llm_decide(ctx)                  # {"action","args"} or {"final": ...}
        if "final" in step:
            return step["final"]
        result = tools[step["action"]](**step["args"])   # execute
        ctx.append(result)                      # observe → append → repeat (ReAct)
    return "stopped: step limit"                # graceful degradation
```
Real: [Anthropic — Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)

### Stage 6 — Multi-agent orchestration
```python
# sketch — supervisor delegates, then synthesizes (only when subtasks are independent)
import asyncio
async def supervisor(task):
    plan = llm_plan(task)                                  # split into subtasks
    results = await asyncio.gather(*[worker(s) for s in plan.subtasks])  # parallel
    return llm_synthesize(task, results)
```
Real: [LangGraph](https://github.com/langchain-ai/langgraph) (state machine) · [CrewAI](https://github.com/crewAIInc/crewAI) (roles) — and *only* for parallel breadth ([ARCHITECTURE-REFERENCES](./ARCHITECTURE-REFERENCES.md))

### Stage 7 — Human-in-the-loop
```python
def decide(claim, threshold=0.8):
    d = agent(claim)
    if d.confidence < threshold or d.is_irreversible:      # uncertainty / risk → gate
        audit_log(d)                                        # trail for later review
        return escalate_to_human(d)                         # approval gate
    return d
```
Real: [HumanLayer](https://github.com/humanlayer/humanlayer) · [LangGraph interrupts](https://github.com/langchain-ai/langgraph)

### Stage 8 — Evaluation + QA
```python
def evaluate(system, dataset):                 # dataset: [(input, gold)]
    hits = sum(system(x) == gold for x, gold in dataset)
    return hits / len(dataset)                 # run N times → report mean ± spread
# LLM-as-judge — sketch: score = judge(f"Is '{out}' correct vs '{gold}'? reply 0 or 1")
```
Real: [DeepEval](https://github.com/confident-ai/deepeval) · [promptfoo](https://github.com/promptfoo/promptfoo) · [Ragas](https://github.com/explodinggradients/ragas)

### Stage 9 — Observability + tracing
```python
# sketch — auto-trace inputs/outputs/latency/cost
# from langfuse.decorators import observe
# @observe()
def step(payload): ...                         # one span per tool call / model call
```
Real: [Langfuse](https://github.com/langfuse/langfuse) · [Phoenix](https://github.com/Arize-ai/phoenix) · [OTel GenAI](https://opentelemetry.io/docs/specs/semconv/gen-ai/)

### Stage 10 — Security + guardrails
```python
import re
INJECT = re.compile(r"ignore (all|previous)|system:|disregard|you are now", re.I)
def guard(text):                               # external text is DATA, not instructions
    return text, (["injection"] if INJECT.search(text) else [])
# also: validate OUTPUT against a schema (Stage 3) + least-privilege tool scopes
```
Real: [OWASP Prompt-Injection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) · [LLM Guard](https://github.com/protectai/llm-guard) · [GUARDRAILS-AND-SECURITY](./GUARDRAILS-AND-SECURITY.md)

### Stage 11 — Production deployment
```dockerfile
# Dockerfile
FROM python:3.12-slim
COPY . /app
RUN pip install -r /app/requirements.txt
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
```yaml
# CI: block the deploy if the eval regresses (your eval set IS the test suite)
- run: python eval.py    # non-zero exit if accuracy < threshold → no ship
```
Real: [vLLM](https://github.com/vllm-project/vllm) · [SGLang](https://github.com/sgl-project/sglang) · [PRODUCTION.md](./PRODUCTION.md)

### Stage 12 — Open source + portfolio
- [ ] Ship a small agent publicly (this repo's [SKILL.md](./SKILL.md) packaging works).
- [ ] Write an architecture doc (steal the shape of [ARCHITECTURE-REFERENCES](./ARCHITECTURE-REFERENCES.md) / a project HLD).
- [ ] Record a 60-sec demo; state the **honest limits** (evals/cost/guardrails).
- [ ] Contribute one PR to a framework you used ([OSS-LANDSCAPE](./OSS-LANDSCAPE.md)).

---
> Build one tiny thing at each stage, then graduate to the decision docs. The stubs are
> the on-ramp; [DECIDE-FIRST](./DECIDE-FIRST.md) + [CHECKLIST](./CHECKLIST.md) are the
> guardrails once it's real.
</content>
