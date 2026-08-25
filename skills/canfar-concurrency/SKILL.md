---
name: canfar-concurrency
description: >
  CANFAR shared /arc/home between sessions, scratch per session, file locking
  on NFS, SQLite pitfalls, AstroAI agent setup locks and Ray control lock.
  Use when two sessions open, concurrent writes, or where chat history lives.
---
# Concurrency & shared home

## Two sessions, one home

Every **interactive** session mounts the **same** `/arc/home/<user>`.
Each session has its **own** `/scratch` — never shared.

| State | Location | Concurrent? |
| --- | --- | --- |
| Config, SSH keys, `~/.canfar` | `/arc/home` | Read-mostly OK |
| Active datasets | `/arc/projects/<group>` | Group POSIX — coordinate |
| Temp I/O | `/scratch` | **Per session only** |
| Vault releases | `vos:…` | ACL-controlled |

**Rule:** teammates share via `/arc/projects` or VOSpace — not `/scratch`.

## NFS / CephFS cautions

- Avoid heavy **SQLite** or lock-heavy apps directly on `/arc/home` — use scratch or project paths with care.
- Use atomic writes (write temp + rename) for config files.
- Large parallel writes to one directory can slow everyone — spread outputs.

## AstroAI images (optional)

| State | Location | Notes |
| --- | --- | --- |
| Agent configs (MCP, skills) | `/arc/home` | Shared |
| Ray control | `~/.astroai/ray` | **One writer** (lock) |
| Agent runtimes (transcripts) | Symlink → scratch | Per-session |

Locks:

| Domain | Lock | Timeout |
| --- | --- | --- |
| Agent setup/plugins/verify | `~/.astroai/lab/agent-setup.lock` | 30 s |
| Ray cluster start/stop | `~/.astroai/ray/control.lock` | 120 s |

**Safe concurrently:** `astroai status`, reads, `canfar ps`.

**Serialize:** `agent setup`, `plugins install`, `verify --fix`, `cluster start`.

`agent setup` symlinks heavy agent stores to scratch so SQLite on NFS does not corrupt.
Chat history on scratch **dies with the session**; configs on `/arc/home` **persist**.

## Agent rules

1. Two sessions = two scratches; share via `/arc/projects` or `vcp`.
2. Permission denied on project file → group membership (`canfar-groups`), not "wrong session".
3. Do not run parallel `verify --fix` in two AstroAI sessions without expecting lock messages.
