---
name: canfar-cli
description: >
  canfar CLI: login logout auth show, create delete ps open sessions, stats
  info, canfar data stage sync, authentication contexts, Science Portal
  alternative. Use when driving CANFAR from terminal or scripting sessions.
---
# CANFAR CLI

Docs: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)

Installed on AstroAI images (`/opt/astroai/venv/cadc`). Upgrade in-session:
`upgrade-cadc-tools.sh --upgrade canfar`.

## Auth

```bash
canfar login
canfar auth show
canfar logout
```

Credentials: `~/.canfar/config.yaml` on `/arc/home` — shared across your sessions.

## Sessions

```bash
canfar create --name myterm webterm
canfar create --name nb --cpu 2 --memory 4 notebook skaha/astroml:latest
canfar ps
canfar open <session-id>
canfar delete <session-id>
canfar stats <session-id>    # when supported
```

AstroAI contributed example:

```bash
canfar create --name dev contributed images.canfar.net/astroai/webterm:26.08
```

## Data movement

```bash
canfar data stage /arc/projects/group/incoming
canfar data sync /scratch/results /arc/projects/group/results
```

For VOSpace URIs use `vcp`/`vls` — see `canfar-vospace`.

## vs astroai

| Tool | Scope |
| --- | --- |
| `canfar` | Platform: auth, sessions, archive data |
| `astroai` | In-session: env, Ray, agents, status |

`astroai status` embeds `canfar auth show` and `canfar ps` when available.

## Agent rules

1. User not on AstroAI image → `canfar` may still work; `astroai` won't.
2. Prefer `canfar ps` over guessing session IDs from URLs.
3. Non-interactive scripts: ensure `canfar login` already done in that home.
