---
name: canfar-cli
description: >
  canfar CLI: login, auth show ls rm, create delete ps open sessions, stats
  info events, canfar data cp ls rm with arc vault local paths, Science Portal
  alternative. Use when driving CANFAR from terminal or scripting sessions.
---
# CANFAR CLI

Docs: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)

Install: platform images include it; locally via `pip install canfar`.

## Auth

```bash
canfar login
canfar login cadc
canfar login srcnet
canfar auth show
canfar auth ls
canfar auth rm cadc          # remove saved credentials for an IDP
canfar auth purge --force    # clear all saved auth
```

Credentials persist under `/arc/home` (e.g. `~/.canfar/config.yaml`) — shared
across your sessions. There is no `canfar logout` — use `auth rm` or `auth purge`.

See `canfar-auth` for IDP, certificates, server selection.

## Sessions

```bash
canfar create notebook skaha/astroml:latest
canfar create --name mydesk --cpu 4 --memory 16 desktop skaha/astroml:latest
canfar ps
canfar ps --json
canfar open <session-id>
canfar delete <session-id>
canfar info <session-id>
canfar events <session-id>
canfar stats
```

Session types: `canfar-sessions`. Batch/headless: `canfar-batch`.

## Data movement (`canfar data`)

Operands use **storage identifier + absolute path** — bare `/arc/...` is rejected:

```bash
canfar data ls -lh arc:/home/$USER
canfar data cp local:/absolute/path/file.fits arc:/projects/mygroup/file.fits
canfar data cp vault:/folder/file.fits arc:/home/$USER/file.fits
```

Inside a session, use POSIX `cp` between `/scratch` and `/arc`. For `vcp`/`vls`
see `canfar-vospace`. Large transfers: `canfar-transfers`.

## Automation

```bash
canfar auth show    # verify before scripts
canfar ps --json    # parse session list
```

Non-interactive jobs: login once in that `/arc/home`, then reuse credentials.

## vs other tools

| Tool | Scope |
| --- | --- |
| `canfar` | Platform: auth, sessions, data |
| `vcp` / Python `vos` | VOSpace |
| `cadcget` | CADC **archives** |
| `astroai` | **AstroAI images only** — env, Ray, agents |

`astroai status` (when present) embeds `canfar auth show` and `canfar ps`.

## Agent rules

1. Prefer `canfar ps` over guessing session IDs from URLs.
2. User on laptop → `canfar login` works; no `/arc` until in a session.
3. Destructive ops (`delete`) — confirm session ID from `canfar ps`.
