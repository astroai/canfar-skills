---
name: canfar-quotas
description: >
  CANFAR storage quotas and allocation: distinguish personal/project POSIX,
  Session ephemeral storage, and VOSpace; inspect live usage with df/du or site
  UI; request more capacity through the deployment process. Use when disk full,
  quota percent, space left, or no space.
---
# Quotas & disk usage

Docs: [Storage overview](https://www.opencadc.org/canfar/latest/platform/storage/)

## Do not guess the quota

CADC commonly provides a small personal home and separately allocated project
space. The current Skaha chart has a 10 GiB first-user storage default and a
200 GiB non-desktop ephemeral-storage ceiling, but operators override these and
desktop scratch uses a different template. Those numbers are implementation
defaults, not the user's live allocation.

## Check usage (all users)

```bash
df -h /arc/home/$USER
df -h /arc/projects/mygroup
du -sh /arc/home/$USER/* 2>/dev/null | sort -h
du -sh /arc/projects/mygroup/* 2>/dev/null | sort -h
```

Vault: [Vault file manager](https://www.canfar.net/storage/vault/list/) usage display.

## Request more space

For CADC, email **`support@canfar.net`** with:
- Project name
- Current usage / quota
- Requested size
- Brief science justification

For SRCNet or another deployment, use the site's project/allocation process.

## Home quota tips

Keep home for:
- `~/.canfar`, `~/.ssh`, configs, small scripts

**Not** for datasets, large environments, or download caches—use project storage
or Session scratch. Substitute the site's persistent path for `/arc`.

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
3. Big science data → **project allocation**, not personal home.

Related: `canfar-storage`, `canfar-groups` (project allocations)
