---
name: canfar-permissions
description: >
  CANFAR permissions: CADC identity, CADC groups, GMS group URIs, project
  allocations on /arc/projects, POSIX ACLs chmod chgrp, Harbor registry access,
  vault VOSpace sharing. Use when access denied, group membership, or creating
  team project space.
---
# Permissions

## Layers

| Layer | Controls |
| --- | --- |
| **CADC identity** | Login across CANFAR, archives, VO |
| **Groups** | Team membership, project access |
| **POSIX ACLs** | Files under `/arc/home`, `/arc/projects` |
| **VOSpace ACLs** | Vault `vos:` read/write/public |
| **Harbor** | Who can pull/push container images |

Group admin: [CADC Group Management](https://www.cadc-ccda.hia-iha.nrc-cnrc.gc.ca/en/groups/)

## Project allocations

`/arc/projects/<name>` is a **VOSpace container with quota + team group** — not a folder you `mkdir`.

Creating one requires **Allocations owner** / admin workflow on the platform.

Inside an existing project:

```bash
ls -la /arc/projects/mygroup/
chmod 664 shared.fits
chgrp mygroup shared.fits
```

## Vault sharing

Web UI: file → Properties → Permissions (owner / group / other × r/w/x).

CLI: `vchmod`, group URIs (`ivo://cadc.nrc.ca/gms?…`) — GMS names appear in `astroai status --json` vault section when relevant.

## AstroAI / Ray

- Ray workers use **your** CANFAR auth from `/arc/home`
- Shared job I/O: paths must be readable by your identity on `/arc/projects/…`

## Agent rules

1. "Permission denied" on `/arc/projects/foo` → check group membership, not sudo.
2. Do not suggest `mkdir /arc/projects/newteam` as project creation.
3. Public data → Vault with explicit other-read; ARC home/projects stay private by default.
