# Phase 3 – Repo Scaffold & Lo-Fi Prototype Guide

*Scope: ≤ 1–2 working days*

---

## 1 Purpose

Turn the Solution Sketch into **runnable code** that demonstrates one end-to-end "happy-path" action, backed by automated tests and CI. Still minimal, still easy to scrap.

---

## 2 Prerequisites

* Phase 2 artifacts committed and reviewed
* `preflight.json` & `solution_sketch.md` locked
* GO decision to proceed

---

## 3 Deliverables

| Artifact                 | Path                                       | Description                                    |
|------------------------|-------------------------------------------|------------------------------------------------|
| **Repo skeleton**        | `/src /tests /planning /spikes`            | Folders + `.gitkeep`                           |
| **Runtime manifest**     | `pyproject.toml` / `package.json`          | Declares dependencies                          |
| **Editor & lint config** | `.editorconfig`, `ruff.toml` / `.eslintrc` | Consistent style                               |
| **CI workflow**          | `.github/workflows/ci.yml`                 | Lint → Tests → Token guard                     |
| **Token guard script**   | `/scripts/token_guard.py`                  | Fails if > cap                                 |
| **Happy-path app**       | `/src/app.py` (or equivalent)              | CLI / web route that achieves first DoD bullet |
| **Tests**                | `/tests/test_app.py`                       | Green on happy path                            |
| **CHANGELOG.md**         | root                                       | Updated on each commit                         |

---

## 4 Time-boxed Schedule (≈ 8 h)

| hh:mm      | Activity                                               |
| ----------- | ------------------------------------------------------ |
| 00:00–01:00 | Init repo, add skeleton & configs                      |
| 01:00–02:00 | Stub CI workflow; include lint + tests (empty)         |
| 02:00–06:00 | Implement happy-path slice via micro-task loop         |
| 06:00–07:00 | Write unit tests; iterate until green                  |
| 07:00–08:00 | Push, tag `v0.2-prototype`, update backlog & changelog |

---

## 5 Repository Scaffold Checklist

* [ ] `src/` contains module folders (≤ 400 LOC each)
* [ ] `tests/` mirrors `src/` structure
* [ ] `planning/` holds Phase docs & backlog
* [ ] `spikes/` stores Phase 1 artifacts
* [ ] `README.md` points to Solution Sketch

### 5.1 Directory tree example

```
├── src
│   ├── orchestrator.py
│   └── __init__.py
├── tests
│   └── test_orchestrator.py
├── planning
│   ├── preflight.json
│   ├── solution_sketch.md
│   └── backlog.tsv
├── scripts
│   └── token_guard.py
├── pyproject.toml
└── .github/workflows/ci.yml
```

---

## 6 CI Workflow Stub (GitHub Actions)

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install -r requirements.txt ruff pytest
      - run: ruff check src tests
      - run: pytest -q
      - run: python scripts/token_guard.py --cap 8000
```

### 6.1 `token_guard.py` snippet

```python
import sys, pathlib, tiktoken
ENC = tiktoken.encoding_for_model("gpt-4o-mini")
cap = int(sys.argv[sys.argv.index('--cap')+1])
paths = pathlib.Path('src').rglob('*.py')
tokens = sum(len(ENC.encode(p.read_text())) for p in paths)
if tokens > cap:
    print(f"Token budget exceeded: {tokens} > {cap}")
    sys.exit(1)
```

---

## 7 Micro-task Loop (repeat per backlog row)

1. **Plan** – select next task (≤ 2 h effort).
2. **Prompt** ChatGPT with micro-task template.
3. **Verify** – run `just ci` or GitHub Action locally.
4. **Commit** & update `CHANGELOG.md`.

---

## 8 Acceptance Checklist

* [ ] CI workflow green locally & on GitHub
* [ ] Happy-path call meets first DoD bullet
* [ ] Tests cover ≥ 60 % of happy-path LOC
* [ ] Token guard passes
* [ ] Backlog refined (no task > 2 h)

---

## 9 Exit Criteria → Phase 4 (“Pain-Driven Upgrades”)

Proceed when:

1. At least 3 alpha users run the prototype and confirm value prop.
2. Recurring pain identified (context bloat, branching, bad JSON, cost).
3. Next phase's single helper library is selected based on that pain.

---

### Remember

> **Ship thin, ship fast, verify everything.**

Phase 3 is about getting *working software* in users' hands while the cost & blast-radius are tiny.
