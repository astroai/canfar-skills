---
name: canfar-concurrency
description: >
  CANFAR shared /arc/home between sessions, scratch per session, astroai agent
  setup locks, Ray control lock, agent runtime symlinks to scratch, SQLite on
  NFS, safe concurrent commands. Use when two sessions open, agent verify
  conflicts, or chat history location.
---
# Concurrency & shared home

## Two sessions, one home

| State | Location | Concurrent? |
| --- | --- | --- |
| Agent configs (MCP, skills, settings) | `/arc/home` | Read-mostly OK |
| Auth (`canfar`, `gh`) | `/arc/home` | Shared — good |
| Env saves (`~/.astroai/lab`) | `/arc/home` | Use locks |
| Ray control (`~/.astroai/ray`) | `/arc/home` | **One writer** (lock) |
| Agent runtimes (transcripts, DBs) | **Symlink → scratch** | Per-session |
| Caches, envs, CLIs | `$SCRATCH` | Per-session |

Each session has **its own** `$SCRATCH` — never shared.

## Locks (astroai)

| Domain | Lock file | Timeout |
| --- | --- | --- |
| Agent setup/plugins/verify | `~/.astroai/lab/agent-setup.lock` | 30 s |
| Ray cluster start/stop | `~/.astroai/ray/control.lock` | 120 s |

Stale lock (dead PID) auto-clears. If blocked: finish the other session's command or wait.

**Safe concurrently:** `astroai status`, `cluster status`, `env export`, reads.

**Serialize:** `agent setup`, `plugins install`, `verify --fix`, `cluster start`.

## Agent runtime relocation

`astroai agent setup` moves heavy agent stores (e.g. `~/.claude/projects`) to
scratch via symlinks so **SQLite on NFS does not corrupt**.

- Chat history on scratch **dies with the session**
- Configs/skills on `/arc/home` **persist**

Dirs >200 MB left in place — `verify --fix` reports them.

## Agent rules

1. Tell users: two webterms = two scratches; share via `/arc/projects`.
2. Do not run `verify --fix` in parallel in two sessions without expecting a lock message.
3. Atomic config writes — never edit JSON MCP files by partial overwrite.
