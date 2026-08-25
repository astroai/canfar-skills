---
name: canfar-quotas
description: >
  CANFAR storage quotas: home directory quota on /arc/home, project quotas on
  /arc/projects, vault VOSpace quota, astroai status --json, CephFS xattrs,
  ceph.dir.rbytes lag, home breakdown, when save or agent setup fails.
  Use when disk full, quota percent, or how much space is left.
---
# Quotas

## Check first

```bash
astroai status
astroai status --json
astroai status --all          # every team project + vault nodes
```

JSON includes: home `quota_pct`, arc project lines, vault nodes, `canfar` auth/ps.

## Home (`/arc/home/<you>`)

- Default allocation ~**10 GB** (CephFS directory quota)
- CANFAR uses **`ceph.quota.max_bytes`** xattrs; `astroai status` prefers these over raw `df`
- **`ceph.dir.rbytes` can lag** a few seconds after large writes — refresh `astroai status`, not a frozen UI bug

**Keep home small:** agent configs, saves, certs — not datasets or conda envs.

```bash
astroai clean --yes           # ~/.cache on home (not scratch caches)
du -sh ~/.cache ~/.local/* 2>/dev/null | sort -h
```

Agent setup **refuses** at home quota ≥98% (use `--force` only if user understands risk).

## Project (`/arc/projects/<group>`)

- Separate team quota; membership via CADC groups
- `astroai status --all` lists each project path + used/total/%

Project allocation is **not** created with `mkdir` — admin/VOSpace allocation required (see `canfar-permissions`).

## Vault

- Per-node quota on VOSpace containers
- Shown in `astroai status --json` under vault when authenticated

## Scratch

- Usually **not** a long-term quota concern — large but **session-only**
- Filling scratch does not free home; filling home blocks saves and agent config

## Agent rules

1. Always run `astroai status --json` before advising "delete X" — cite actual `quota_pct`.
2. Warn when home >90%: agent configs and `astroai save` may fail.
3. Move big data to `/arc/projects` or `$SCRATCH`, never bloat home.
