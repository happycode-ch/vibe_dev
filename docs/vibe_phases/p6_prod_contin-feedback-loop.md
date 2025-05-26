# Phase 6 – Production & Continuous Feedback Guide

*Scope: 1–2 weeks (overlapping with live usage)*

> **Goal:** Launch the hardened beta into production, establish a feedback‑driven improvement loop, and prepare the system for scale and maintenance.

---

## 1 Prerequisites

* Phase 5 beta has run ≥ 48 h in staging with no P1 bugs
* Success metric met by ≥ 5 beta users
* Stakeholders sign off on launch checklist

---

## 2 Key Objectives

1. **Production‑grade infrastructure** – resilient deploy, rollback, secret rotation.
2. **Operational excellence** – SLOs, on‑call alerts, runbooks.
3. **User feedback channels** – in‑app reporting, analytics, support pipeline.
4. **Continuous delivery** – trunk‑based dev, feature flags, canary releases.
5. **Post‑launch iteration cadence** – weekly metrics review + backlog grooming.

---

## 3 Deliverables

| Artifact            | Path / Tool                             | Description                                   |
| ------------------- | --------------------------------------- | --------------------------------------------- |
| **Prod infra code** | `infra/prod/` (Terraform, Pulumi, Fly…) | HA replicas, autoscaling, secrets manager     |
| **CD pipeline**     | `.github/workflows/prod.yml` or ArgoCD  | Build → test → canary → promote               |
| **Runbooks & SLOs** | `/ops/runbooks/*.md`                    | e.g. ≥ 99.5 % success rate, < 2 s p95 latency |
| **Alerting rules**  | Grafana/LangSmith alerts                | PagerDuty / Slack integration                 |
| **User analytics**  | PostHog / Amplitude config              | Funnels, session heatmaps                     |
| **Feedback board**  | GitHub Discussions / Linear             | Triage queue mapped to backlog IDs            |
| **Launch post**     | `/docs/launch_<date>.md`                | Announces v1, links docs & support            |

---

## 4 Time‑boxed Launch Plan

| Day      | Focus                | Checklist                                                                     |
| -------- | -------------------- | ----------------------------------------------------------------------------- |
| **1**    | Infra hardening      | - Provision prod cluster ‑ replicas + autoscale<br>- Enable HTTPS, certs, WAF |
| **2**    | CD pipeline          | - Build artifact signing<br>- Canary rollout script & auto‑rollback           |
| **3**    | Observability        | - Dashboards: error, latency, token cost<br>- SLA alerts wired to pager       |
| **4**    | Analytics & Feedback | - Integrate PostHog/Amplitude SDK<br>- In‑app “Send feedback” hook            |
| **5**    | Runbooks & SLOs      | - Doc triage, restart, rollback steps<br>- PDF share with on‑call             |
| **6**    | Launch window        | - Freeze code, flip feature flag to 100 %<br>- Monitor 2 h —> declare GA      |
| **7–10** | Early‑life support   | - Daily metric check‑ins<br>- Triage feedback → backlog                       |

---

## 5 Operational Playbook (Excerpt)

```markdown
### Service SLOs
| Metric | Target | Alert threshold |
|--------|--------|-----------------|
| p95 latency | ≤ 2 s | 2.5 s for 5 m |
| Error rate | ≤ 1 % | 3 % for 5 m |
| Token cost / req | ≤ $0.002 | $0.003 avg 1 h |

### Incident Workflow
1. **Detect** – Grafana alert fires (#incident‑bot).
2. **Assign** – On‑call acknowledges in PagerDuty.
3. **Mitigate** – Rollback via `gh workflow run rollback.yml`.
4. **Communicate** – Status page update within 15 m.
5. **Post‑mortem** – Template in `/ops/postmortems/YYYY‑MM‑DD.md`.
```

---

## 6 Acceptance Checklist

* [ ] Production deploy passes canary & promoted to 100 % traffic
* [ ] All SLO dashboards green for first 24 h
* [ ] Feedback mechanism captures ≥ 10 user submissions
* [ ] Continuous delivery pipeline merges → prod ≤ 30 min w/o downtime
* [ ] Runbooks tested via simulated incident

---

## 7 Continuous Feedback Loop

1. **Weekly Metrics Review (30 min)**

   * Latency, error, cost, guardrail triggers, user funnels.
2. **Backlog Grooming (30 min)**

   * Prioritise feedback items vs roadmap.
3. **Iteration Sprint (1 week)**

   * Ship top 1–3 improvements, verify via A/B or metrics.
4. **Quarterly Retrospective**

   * Re‑evaluate value prop, success metric, stack choices.

---

## 8 Exit Criteria → Maintenance Mode

Project transitions to steady‑state when:

* SLOs met 90 % of weeks in a quarter
* Token cost variance < ±10 % month‑on‑month
* Feature velocity stabilises to planned cadence

---

### Remember

> **Production is a process, not a milestone.** Automate, observe, learn, iterate.
