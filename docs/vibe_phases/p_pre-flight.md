# AI‑First Project Pre‑flight Guide

A 15‑item checklist and ready‑to‑use templates to lock scope, cost and guard‑rails **before** your first AI‑generated line of code.

---

## 1. Pre‑flight Checklist

| #  | Item to lock            | Key questions                     | Concrete artefacts                  |
| -- | ----------------------- | --------------------------------- | ----------------------------------- |
| 1  | **Value Proposition**   | What pain & gain?                 | 1‑sentence statement                |
| 2  | **Success Metric**      | How will we measure "done"?       | Quant target (e.g. "≤5 min claim")  |
| 3  | **Primary User Flow**   | Steps from problem → success?     | Napkin diagram / markdown list      |
| 4  | **Data Sources**        | Which docs / APIs feed the model? | Source list + access status         |
| 5  | **Constraints**         | Budget, latency, compliance?      | Table in `constraints.md`           |
| 6  | **Definition of Done**  | Binary acceptance criteria?       | ≤3 bullets per feature              |
| 7  | **Runtime Stack**       | Language, hosting, CI lane?       | `why_<lang>.md`                     |
| 8  | **Glue Library**        | One helper lib only?              | Choice note (e.g. "LangGraph v0.6") |
| 9  | **Token Budget Guard**  | Hard caps per call / day?         | Entry in `constraints.md`           |
| 10 | **Testing Harness**     | Unit, lint, token check?          | Installed & green                   |
| 11 | **Repo Skeleton**       | Folder map committed?             | `/src /tests /planning` etc.        |
| 12 | **Prompt Style Guide**  | Micro‑task rules?                 | `prompt_style.md`                   |
| 13 | **Rollback Plan**       | How to undo AI mass edits?        | Git branch model / flags            |
| 14 | **Stakeholder Cadence** | Demo interval?                    | Calendar entry                      |
| 15 | **OOS Keyword**         | AI signal for scope violation?    | Added to Style Guide                |

Tick every box **before** you start prompting for code.

---

## 2. Pre‑flight JSON (fill & commit as `/planning/preflight.json`)

```json
{
  "value_prop": "",
  "success_metric": "",
  "user_flow": "",
  "data_sources": [],
  "constraints": {
    "budget_usd_month": 0,
    "latency_s": 0,
    "compliance": []
  },
  "definition_of_done": [],
  "runtime_stack": { "lang": "", "hosting": "" },
  "glue_library": "",
  "token_caps": { "per_call": 0, "per_day": 0 },
  "testing": ["pytest", "ruff", "token_log"],
  "repo_skeleton_committed": false,
  "prompt_style": "micro + code-block-only",
  "rollback_strategy": "git_branches",
  "cadence": "daily_demo",
  "oos_keyword": "OOS"
}
```

---

## 3. Next Phase Roadmap (once checklist = ✅)

1. **Feasibility Spike (≤1 day)** – prove metric with raw SDK + single prompt.
2. **Solution Sketch (≤1 day)** – white‑board data & API flow.
3. **Repo Scaffold** – commit skeleton & CI harness.
4. **Lo‑fi Prototype (≤1 week)** – one "happy path" CLI / webform.
5. **Alpha Feedback** – 3‑5 users; log fails, costs.
6. **Pain‑Driven Upgrades** – add exactly *one* helper lib per repeating pain:

   * Context bloat → LlamaIndex
   * Branch logic → LangGraph
   * Bad JSON / PII → Guardrails‑AI
   * Unknown spend → LangSmith / LangFuse

---

## 4. Micro‑task Prompt Template (paste in each new chat)

```text
Context: {1‑sentence value prop}
Task #{ID} – {Title}
Definition of Done:
- {bullet 1}
- {bullet 2}
Constraints:
- Edit only: {paths}
- Additions ≤ {N} LOC
Return a single ```{lang}``` block and nothing else.
```

---

### Usage

1. Duplicate the JSON, fill every field, commit.
2. Confirm all 15 checklist items are green.
3. Start ChatGPT with the Micro‑task template.
4. Verify via tests & token guard before committing.

**Locked pre‑flight ⇒ AI stays on rails. Happy shipping!**
