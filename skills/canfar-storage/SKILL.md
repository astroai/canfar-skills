---
name: canfar-storage
description: >
  CANFAR storage: /scratch session SSD, /arc/home personal POSIX, /arc/projects
  team POSIX, default quotas, persistence, scratch vs arc workflow, SSHFS. Use when
  asking where to save files, sharing between sessions, scratch vs arc, data layout.
---
# CANFAR storage

Docs: [Storage systems](https://opencadc.github.io/canfar/latest/platform/storage/)

## Official tiers

| Storage | Path | Default quota | Lifetime | Shared? |
| --- | --- | --- | --- | --- |
| **Scratch** | `/scratch` | ~200 GB / session | Session | **No** |
| **ARC Home** | `/arc/home/<user>` | ~10 GB | Permanent | Yes (your sessions) |
| **ARC Projects** | `/arc/projects/<group>` | ~200 GB+ (varies) | Permanent | **Yes** (group) |
| **Vault** | `vos:…` | project-dependent | Permanent | VOSpace ACLs |

## Decision guide

```text
Teammate needs live access?     → /arc/projects/…
Large temp I/O this session?    → /scratch
Config, certs, small dotfiles?  → /arc/home (keep small)
Long-term archive / public URL? → Vault (canfar-vospace)
```

## Session workflow

```bash
cp /arc/projects/mygroup/raw/big.fits /scratch/
pixi run python analyze.py /scratch/big.fits
cp results.csv /arc/projects/mygroup/results/
```

**Scratch is wiped** when the session ends.

## Suggested project layout

```text
/arc/projects/<group>/
├── raw/          # incoming data
├── working/      # intermediate
├── results/      # outputs
├── scripts/      # code
└── docs/         # README, procedures
```

## External access (SSHFS)

From your laptop, mount ARC via SSH — keys in `/arc/home/<user>/.ssh/authorized_keys`.
Details: [Filesystem access](https://opencadc.github.io/canfar/latest/platform/storage/filesystem.md)

## AstroAI note

On AstroAI images: `$WORK` = `$SCRATCH/src` — project code survives container OOM.
Agent CLIs on `$SCRATCH`; configs on `/arc/home`. See `canfar-concurrency`.

## Commands

```bash
df -h /arc/home/$USER
df -h /arc/projects/mygroup
du -sh /arc/projects/mygroup/* | sort -h
```

Quota increase: email `support@canfar.net` — see `canfar-quotas`.

Related: `canfar-transfers`, `canfar-vospace`, `canfar-groups`
