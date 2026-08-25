---
name: canfar-sessions
description: >
  CANFAR interactive sessions: create, open, stop, lifecycle, idle suspend,
  flexible vs fixed CPU/memory, Science Portal vs canfar CLI, AstroAI images
  (webterm, notebook, marimo, vscode, openresearch, ray-manager). Use when
  launching, managing, or understanding session behavior or image choice.
---
# CANFAR sessions

## Create and manage

**Portal:** [Science Portal](https://www.canfar.net/science-portal/) → pick image → launch.

**CLI:**

```bash
canfar login                              # once; creds under /arc/home
canfar create --name demo webterm         # contributed image
canfar create --cpu 4 --memory 8 notebook skaha/astroml:latest   # fixed resources
canfar ps
canfar open <session-id>
canfar delete <session-id>
```

## AstroAI images (Contributed unless noted)

| Image | Port | Use |
| --- | --- | --- |
| `openresearch` | 5000 | Day-to-day hub + `/astroai-agents/` |
| `webterm` / `ghostty-web` | 5000 | Shell + tmux |
| `vscode` | 5000 | OpenVSCode |
| `marimo` | 5000 | Reactive `.py` notebooks |
| `notebook` | 8888 | JupyterLab |
| `ray-manager` | 5000 | Ray cluster UI + head |
| `improc-webterm` / `improc-notebook` | 5000 / 8888 | Imaging stack |

Tag example: `images.canfar.net/astroai/openresearch:26.08`

```bash
canfar create --name orx contributed images.canfar.net/astroai/openresearch:26.08
```

## Lifecycle

1. **Create** — 30 s–3 min (image pull on first use)
2. **Active** — full CPU/RAM within allocation; storage mounted
3. **Idle** — may suspend after inactivity (platform policy)
4. **Delete** — container gone; **data on `/arc` and VOSpace persists**

!!! Persist before delete: `git push`, `astroai save`, `canfar data sync`.

## Resource modes

| Mode | When | CLI hint |
| --- | --- | --- |
| **Flexible** (default) | Exploration, dev | `canfar create notebook …` |
| **Fixed** | Predictable performance | `--cpu N --memory GiB` |

`astroai status` shows **this pod's** CPU/mem/cgroup — not cluster-wide Ray.

## OpenResearch hub

On `openresearch`: connect URL + `/astroai-agents/` → **Start batch compute**
(autoscaling ray-manager) + agent install table.

## Agent rules

1. Session containers are **temporary**; only `/arc`, VOSpace, and git remotes survive.
2. `/scratch` is **not visible** to other sessions — not a bug.
3. Headless **Pending** on contributed sessions often means quota (~3) or Skaha flake — see `canfar-troubleshooting`.
