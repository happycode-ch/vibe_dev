# Phase 1 – Feasibility Spike Guide

*Scope: ≤ 1 working day*

---

## 1 Purpose

Prove—quickly and cheaply—that an LLM plus **one prompt** (and optionally **one helper library**) can hit **≥ 60 %** of the success metric defined in `preflight.json`.

---

## 2 Inputs (locked before you start)

* `preflight.json` – value_prop, success_metric, data_sources, constraints
* Raw sample data / docs (if any)
* Working API key for chosen model (e.g. `OPENAI_API_KEY`)

---

## 3 Time‑boxed Schedule (≈ 6 hours)

| hh:mm      | Activity                                                                           |
| ----------- | ---------------------------------------------------------------------------------- |
| 00:00–00:30 | Re‑read **Success Metric** & select smallest "happy‑path" user action to automate  |
| 00:30–01:00 | Draft single prompt in ChatGPT; iterate until answer quality ≥ 60 %                |
| 01:00–02:30 | Code throw‑away script `spike.py` (or `spike.ts`) using raw SDK + prompt           |
| 02:30–03:00 | Run on ≥ 5 varied inputs; record token, latency, quality notes                     |
| 03:00–04:00 | *Optional*—Integrate **one** helper lib if blocked (e.g. LlamaIndex for long docs) |
| 04:00–05:00 | Write quick‑and‑dirty test `test_spike.py` validating metric                       |
| 05:00–05:30 | Summarise findings into `/planning/spike_report.md`                                |
| 05:30–06:00 | Go/No‑Go meeting (yourself + any stakeholder)                                      |

---

## 4 Spike Deliverables

* **`spike.py`** – minimal script or Jupyter cell performing the task
* **`test_spike.py` / `test_spike.ts`** – asserts metric ≥ 60 %
* **`token_log.tsv`** – columns: input, prompt_tokens, completion_tokens, cost_usd
* **`/planning/spike_report.md`** – template below

### 4.1 `spike_report.md` template

```markdown
### Feasibility Spike Report – {date}

**Goal / Metric**
- {success_metric}

**Approach**
- Model: {gpt‑4o‑mini}
- Helper libs: {none | llama‑index@0.10}
- Prompt length: {N} tokens

**Results (5 samples)**
| Case | Metric % | Latency s | Cost USD |
|------|----------|----------|----------|
|      |          |          |         |

**Average metric:** {avg}%
**Average cost per call:** ${cost}

**Decision**
- [ ] GO – proceed to Phase 2 (Solution Sketch)
- [ ] NO‑GO – pivot / rethink

**Reasons**
- …
```

---

## 5 Micro‑prompt template for the spike

```text
Act as a domain expert. Goal: {value_prop}. Success metric: {metric_description}.
Task: Given {minimal input}, produce {desired output}.
Return only the output, no commentary.
```

---

## 6 Evaluation & Decision

> *GO* criteria_—≥ 60 % metric success **and** cost < 10 % of monthly budget cap.

If GO → commit spike artefacts under `/spikes/` and move to **Phase 2 – Solution Sketch**.

If NO‑GO → revisit value_prop or explore alternate approach (different model, structured tool chain).

---

## 7 Handover to Phase 2

Prepare:

1. Extract the **prompt** and **findings** from `spike_report.md`.
2. Draft high‑level data/API flow diagram.
3. Update backlog.tsv with tasks uncovered.
   Then invoke the **Solution Sketch Guide** (next document).
