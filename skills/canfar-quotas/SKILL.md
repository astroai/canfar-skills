---
name: canfar-quotas
description: >
  CANFAR storage quotas: home 10GB, project and scratch defaults, vault VOSpace
  quota, df du monitoring, request increase support@canfar.net, astroai status
  on AstroAI images. Use when disk full, quota percent, space left, no space.
---
# Quotas & disk usage

Docs: [Storage overview](https://opencadc.github.io/canfar/latest/platform/storage/)

## Default allocations (typical)

| Tier | Path | Typical quota |
| --- | --- | --- |
| ARC Home | `/arc/home/<user>` | **~10 GB** |
| ARC Project | `/arc/projects/<group>` | **~200 GB** (varies) |
| Scratch | `/scratch` | **~200 GB** / session |
| Vault | `vos:…` | Per container |

## Check usage (all users)

```bash
df -h /arc/home/$USER
df -h /arc/projects/mygroup
du -sh /arc/home/$USER/* 2>/dev/null | sort -h
du -sh /arc/projects/mygroup/* 2>/dev/null | sort -h
```

Vault: [Vault file manager](https://www.canfar.net/storage/vault/list/) usage display.

## Request more space

Email **`support@canfar.net`** with:
- Project name
- Current usage / quota
- Requested size
- Brief science justification

## Home quota tips

Keep home for:
- `~/.canfar`, `~/.ssh`, configs, small scripts

**Not** for datasets, conda envs, or download caches — use `/arc/projects` or `/scratch`.

## Scratch

Full scratch ≠ home full. Scratch resets each session — delete temp files freely.

## AstroAI images (optional)

```bash
astroai status --json   # home %, project lines, vault nodes, ceph xattrs
astroai clean --yes     # ~/.cache on home only
```

Ceph `ceph.dir.rbytes` may lag seconds after large writes — refresh status.

Agent setup may refuse at home ≥98% full.

## Agent rules

1. Run `df`/`du` before delete advice — cite paths and sizes.
2. Warn at >90% home: saves and logins may fail.
3. Big science data → **project space**, not home.

Related: `canfar-storage`, `canfar-groups` (project allocations)
