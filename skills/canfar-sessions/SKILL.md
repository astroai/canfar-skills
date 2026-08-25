---
name: canfar-sessions
description: >
  CANFAR interactive sessions: Notebook JupyterLab, Desktop GUI, CARTA radio,
  Firefly tables, Contributed apps, headless batch, flexible vs fixed resources,
  GPU, session lifecycle, Science Portal and canfar create. Use when launching
  sessions, choosing session type, CARTA Firefly desktop notebook.
---
# Interactive sessions

Docs: [Sessions overview](https://opencadc.github.io/canfar/latest/platform/sessions/)

## Session types

| Type | Interface | Best for |
| --- | --- | --- |
| **Notebook** | JupyterLab :8888 | Analysis, docs, prototyping |
| **Desktop** | Full Linux GUI | CASA, legacy GUI tools |
| **CARTA** | Radio astronomy viewer | Cubes, masks, regions |
| **Firefly** | Table/image viewer | Survey catalogs |
| **Contributed** | Community web apps | marimo, VS Code web, custom |
| **Headless** | No UI | Batch — see `canfar-batch` |

Launch: Science Portal (your deployment's URL) or `canfar create`.

## Lifecycle

- **Start:** ~30 s–3 min (image pull first time)
- **Runtime:** deployer-configured (`expirySeconds` in Skaha helm). Chart default **4 days** for interactive; CADC FAQ cites up to **7 days** — verify on your site. Headless default **14 days** in stock chart.
- **End:** container deleted — **data on `/arc` and Vault persists**

Always persist to `/arc/projects` or `vos:` before delete.

## Session count limits

Interactive sessions (notebook, desktop, CARTA, Firefly, contributed) share a
**per-user cap** (stock helm default **5**). Headless/batch is **exempt**.
At cap, **new creates are rejected** — delete idle sessions:

```bash
canfar ps
canfar delete <session-id>
```

## Resources

| Mode | When |
| --- | --- |
| **Flexible** (default) | Exploration, variable load |
| **Fixed** (`--cpu`, `--memory`) | Predictable performance, deadlines |

```bash
canfar create notebook skaha/astroml:latest
canfar create --cpu 4 --memory 16 desktop skaha/astroml:latest
```

## GPU

Request GPU in session config; use CUDA-enabled images (e.g. `astroml-cuda`).
Verify with `nvidia-smi` when allocated.

## Contributed applications

Portal → **Contributed** → pick app. Web UI must listen on **port 5000**
(Skaha probe contract). Community guide: [Contributed apps](https://opencadc.github.io/canfar/latest/platform/sessions/contributed.md)

## AstroAI images (optional subset)

When your site ships AstroAI Harbor catalog:

| Image | Notes |
| --- | --- |
| `openresearch` | Hub at `/astroai-agents/` |
| `webterm`, `vscode`, `marimo`, `notebook` | See `/opt/astroai/USAGE.md` |
| `ray-manager` | With `astroai-ray` |

Generic CANFAR users use `skaha/*` and team images from Harbor.

## Agent rules

1. `/scratch` invisible to other sessions — not a bug.
2. Match session type to workflow (CARTA for radio cubes, not Notebook alone).
3. Pending after create → `canfar events <id>`, not just "wait" (queue, pull, probe).

Related: `canfar-containers`, `canfar-limits`, `canfar-cli`
