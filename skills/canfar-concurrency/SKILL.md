---
name: canfar-concurrency
description: >
  CANFAR concurrent Sessions: shared persistent personal/project POSIX storage,
  per-Session scratch, atomic writes and lock-heavy database pitfalls, plus
  optional AstroAI setup/Ray locks. Use when two Sessions write concurrently,
  files appear inconsistent, or the user asks what state is shared.
---
# Concurrency & shared home

## Two sessions, one home

At CADC, every interactive Session mounts the same `/arc/home/<user>`. Other
deployments commonly mount the same persistent home below `/cavern`. Each
Session has its own `/scratch`; use the live mount paths.

| State | Location | Concurrent? |
| --- | --- | --- |
| Config, SSH keys, `~/.canfar` | `/arc/home` | Read-mostly OK |
| Active datasets | `/arc/projects/<group>` | Group POSIX — coordinate |
| Temp I/O | `/scratch` | **Per session only** |
| Vault releases | `vos:…` | ACL-controlled |

**Rule:** teammates share via `/arc/projects` or VOSpace — not `/scratch`.

## Shared POSIX filesystem cautions

- The backing filesystem is deployment-specific. Avoid heavy **SQLite** or
  lock-heavy apps on shared personal storage; use local scratch for active DB
  state and copy durable exports/checkpoints back safely.
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
2. Permission denied on a project file → inspect identity, group, allocation, and POSIX mode.
3. Do not run parallel `verify --fix` in two AstroAI sessions without expecting lock messages.
