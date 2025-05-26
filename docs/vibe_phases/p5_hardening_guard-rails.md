# Phase 5 – Hardening & Guardrails Guide

*Scope: 2–4 working days*

> **Goal:** Move from alpha prototype to a **beta‑ready build** that survives edge‑cases, bad inputs, malicious prompts, and real‑world load—while keeping cost predictable.

---

## 1 Prerequisites

* Phase 4 upgrade stable for ≥ 24 h and metrics improved
* Backlog contains only bugs / polish, no > 2 h tasks
* Stakeholders agree feature set is “MVP‑complete”

---

## 2 Key Objectives

1. **Robust I/O contracts** – every LLM call wrapped by JSON schema validation & retry logic.
2. **Automated evaluation** – offline datasets measuring accuracy, toxicity, PII leakage.
3. **Observability dashboards** – cost, latency, error rate, guardrail triggers.
4. **Security & compliance** – secrets management, audit logging, dependency scanning.
5. **Staging deployment** – one‑click build mirroring prod infra.

---

## 3 Deliverables

| Artifact                         | Path                                     | Description                                              |
| -------------------------------- | ---------------------------------------- | -------------------------------------------------------- |
| **Guardrail policies**           | `/guardrails/`                           | YAML / JSON schemas for every model call                 |
| **Eval dataset**                 | `/eval/`                                 | TSV/JSONL with ≥ 50 edge‑case prompts + expected outputs |
| **Eval harness**                 | `/scripts/run_eval.py`                   | Uses TruLens, RAGAS, or custom metrics                   |
| **LangSmith / LangFuse project** | external                                 | Live traces & dashboards                                 |
| **Security config**              | `.github/dependabot.yml`,  Snyk/OSS scan | Auto PR for vuln upgrades                                |
| **Staging infra**                | `infra/terraform/` or `fly.toml`         | Replicates prod settings                                 |
| **Beta release tag**             | `v0.4-beta`                              | Signed & changelogged                                    |

---

## 4 Time‑boxed Schedule

| Day   | Focus                    | Checklist                                                                                              |
| ----- | ------------------------ | ------------------------------------------------------------------------------------------------------ |
| **1** | Guardrails integration   | ‑ Wrap each `/src/` LLM helper<br>‑ Add retry w/ exponential backoff<br>‑ Tests for success/fail paths |
| **2** | Evaluation harness       | ‑ Build dataset (edge + adversarial)<br>‑ Implement `run_eval.py`<br>‑ Fail CI if score < threshold    |
| **3** | Observability + Security | ‑ Wire LangSmith/LangFuse keys<br>‑ Create Grafana or built‑in dashboards<br>‑ Enable Dependabot/Snyk  |
| **4** | Staging deploy & soak    | ‑ Provision staging<br>‑ Smoke tests under load (Locust/K6)<br>‑ Fix regressions, tag beta             |

*Time‑boxes may compress if parts already exist.*

---

## 5 Guardrails Playbook

1. **Choose policy engine** – Guardrails‑AI, NeMo, or Rebuff.
2. **Write schema** for each endpoint (JSON shape, enums, ranges).
3. **Set retry budget** – e.g., 2 attempts then safe fallback.
4. **Unit test** with invalid / malicious inputs.
5. **Log trigger counts** to observability layer.

---

## 6 Evaluation Harness Template (Python)

```python
from trulens_eval import TruLlama
from eval.dataset import load_cases
from src.app import generate

evaluator = TruLlama(metrics=["toxicity", "groundedness", "json_schema"],
                     dataset=load_cases("/eval/cases.jsonl"))
score = evaluator.evaluate(generate)
assert score.overall > 0.8, "Eval threshold not met"
```

Add this script to CI; fail build if assertion trips.

---

## 7 Acceptance Checklist

* [ ] ≥ 95 % LLM outputs validate against schemas without retry
* [ ] Offline eval score ≥ target; CI gate active
* [ ] Dashboards show cost, latency, guardrail triggers in real‑time
* [ ] No critical vulnerabilities in dependency scan
* [ ] Staging soak test (≥ 1 h, 100 RPS) passes with < 1 % error rate
* [ ] `v0.4-beta` tag pushed and deployed to staging

---

## 8 Exit Criteria → Phase 6 (“Production & Feedback Loop”)

Proceed when:

1. Beta build runs 48 h in staging with no P1 bugs.
2. Stakeholders sign off on guardrail coverage & metrics.
3. At least 5 beta users confirm the app meets the success metric.

---

### Remember

> **Beta ≠ final**—but it must be safe. Hardening is cheaper now than post‑launch.
