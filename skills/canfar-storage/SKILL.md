---
name: canfar-storage
description: >
  CANFAR storage layout: /scratch session SSD, /arc/home personal POSIX,
  /arc/projects team POSIX, WORK and SRCDIR on AstroAI, what persists across
  sessions, where to put code vs data vs caches, canfar data sync. Use when
  the user asks where to save files, sharing between sessions, or scratch vs arc.
---
# CANFAR storage

## Tier table (memorize)

| Tier | Path | Lifetime | Shared? | Use for |
| --- | --- | --- | --- | --- |
| **Scratch** | `$SCRATCH`, `/scratch` | Session pod | **No** | Large temp data, caches |
| **Source/work** | `$WORK`, `$SRCDIR` | Session (OOM-safe on CANFAR) | **No** | Code, pixi/uv projects |
| **Home** | `/arc/home/<you>`, `$HOME` | Persistent | **Yes** | Config, certs, small saves |
| **Projects** | `/arc/projects/<group>` | Persistent | **Yes** (group) | Team data, shared I/O |
| **Vault** | `vos:…` | Persistent | VOSpace ACLs | Archive, public share |

On CANFAR AstroAI: **`$WORK` = `/scratch/src`** — same device as scratch but
relocated so container OOM does not wipe source tree.

## Decision guide

```text
Teammate needs the file while sessions run?  → /arc/projects/…
Only you, this session, big/fast?            → $SCRATCH
Config, gh auth, agent MCP, astroai saves?   → /arc/home (keep small)
Long-term archive or public URL?             → Vault / vos:
Ray job input/output?                        → /arc (not scratch)
```

## Commands

```bash
astroai status --json          # paths, scratch mount, home quota
astroai env export             # WORK, SCRATCH, ASTROAI_LAB_BIN_DIR

canfar data stage /arc/projects/mygroup/raw
canfar data sync /scratch/out /arc/projects/mygroup/out

vls vos:myuser
vcp local.fits vos:myuser/data/
```

## Home vs scratch (AstroAI agents)

| Put here | Examples |
| --- | --- |
| `$HOME` (/arc) | `~/.cursor`, `~/.astroai/lab`, `canfar` auth, `gh` |
| `$SCRATCH` | Agent CLI binaries, pixi/uv caches, big downloads |
| `$WORK` | Project repo, `pixi.toml`, `.venv` |

**Do not** fill home with pip/conda caches — use project env under `$WORK` or
scratch-backed tool dirs.

## Agent rules

1. Never assume `/scratch` from session A is visible in session B.
2. Before session ends: `astroai save`, `git push`, or sync to `/arc` / `vos:`.
3. `--input` / `--output` on Ray jobs are **URIs only** — data must already be on `/arc`.
