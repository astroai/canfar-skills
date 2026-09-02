---
name: ml-experimentation
description: >
  Machine learning and data-science experiments — tabular, time series, or
  multi-table data. Use when the user explores a dataset, checks data quality,
  builds or trains a classifier/regressor, sets up sklearn or skrub pipelines,
  evaluates metrics, cross-validates, compares models, organizes an ML project,
  bootstraps a pixi/uv Python env for ML, iterates on experiments, audits
  results with skore, or asks in plain language: train a model, baseline
  pipeline, how good is this model, explore my data, feature engineering,
  predict X from Y, classification, regression, machine learning, tabular ML,
  skore report, next experiment. Students do not need to name Probabl skills —
  read the matching skill below and follow it.
---
# ML experimentation (intent router)

Students describe goals in plain language. **Do not ask them to pick a skill
name.** Read the Probabl skill that matches their intent (under
`~/.cursor/skills/` when `npx skills add probabl-ai/skills` has been run).

## Route by intent

| User is trying to… | Read skill |
| --- | --- |
| Understand data, distributions, leakage, dtypes, missing values | `explore-ml-data` |
| Declare pipeline steps (load → preprocess → estimator), skrub DataOps | `build-ml-pipeline` |
| Fit/evaluate, metrics, plots, skore reports, CV | `evaluate-ml-pipeline` |
| Statistical + functional tests before shipping | `test-ml-pipeline` |
| Stress-test on future/unseen data shapes | `smoke-test-ml-pipeline` |
| Post-run audit from a skore report | `audit-ml-pipeline` |
| Track experiments, journal, design next run | `iterate-ml-experiment` |
| Diagnostics from skore → next experiment | `iterate-from-skore` |
| User proposes idea, paper URL, or reference repo | `iterate-from-user` |
| Project layout, cookiecutter-style workspace | `organize-ml-workspace` |
| Python style, typing, ruff-friendly patterns | `python-code-style` |
| pixi / uv / venv bootstrap for the project | `python-env-manager` |
| Which library for which job (pandas, polars, sklearn, …) | `data-science-python-stack` |
| Look up an installed package’s public API | `python-api` |

If several rows apply, follow pipeline order: **explore → build → evaluate →
test → iterate**. Do not skip explore when data is new.

## AstroAI conventions

- Project env: **pixi** or **uv** under `${WORK}` — not `$HOME`.
- Ray jobs: `astroai cluster start` then `astroai run script.py`.
- Skore CLI (optional): `skore skills list` · `skore skills install` — install
  via `astroai agent plugins install skore-cli`.

## Missing skills?

If Probabl skills are not installed:

```bash
npx skills add probabl-ai/skills
astroai agent plugins install skore-cli   # optional CLI for skore skills hub
```
