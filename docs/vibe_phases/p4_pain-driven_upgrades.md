# Phase 4 – Pain-Driven Upgrades Guide

*Scope: 1–2 weeks (iterative)*

> **Goal:** Add exactly one helper library per recurring pain point, while maintaining the system's simplicity and keeping costs predictable.

---

## 1 Prerequisites

* Phase 3 prototype has been used by 3–5 alpha users
* Clear pain points documented in `/planning/pain_log.md`
* Success metric still being tracked and logged

---

## 2 Key Objectives

1. **Identify recurring pain** – document frequency and impact
2. **Select minimal helper** – one library per pain point
3. **Measure improvement** – before/after metrics
4. **Maintain simplicity** – no premature optimization

---

## 3 Common Pain Points & Solutions

| Pain Point | Helper Library | When to Add |
|------------|----------------|-------------|
| Context bloat | LlamaIndex | When context > 8K tokens |
| Branch logic | LangGraph | When flow has > 3 branches |
| Bad JSON/PII | Guardrails-AI | When validation fails > 5% |
| Unknown spend | LangSmith | When cost variance > 20% |

---

## 4 Time-boxed Schedule

| Day | Focus | Checklist |
|-----|-------|-----------|
| **1** | Pain analysis | - Review pain_log.md<br>- Quantify impact<br>- Select first helper |
| **2–3** | First helper | - Install & configure<br>- Update tests<br>- Measure improvement |
| **4–5** | Second helper | - Repeat for next pain point<br>- Keep changes isolated |
| **6–10** | Iterate | - Add helpers one at a time<br>- Verify metrics improve |

---

## 5 Acceptance Checklist

* [ ] Each helper library added in isolation
* [ ] Success metric improved by ≥ 10%
* [ ] Token cost variance < 20%
* [ ] Test coverage maintained
* [ ] Documentation updated

---

## 6 Exit Criteria → Phase 5 (Hardening)

Proceed when:

1. All major pain points addressed
2. Success metric stable for 48h
3. No new pain points emerging

---

### Remember

> **Add complexity only when pain demands it.** Each helper library should solve a clear, recurring problem.
