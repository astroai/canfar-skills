---
name: canfar-permissions
description: >
  CANFAR permissions beyond groups: project allocations, POSIX ACLs chmod chgrp,
  Harbor registry push pull, API authentication, vault VOSpace ACLs, external
  collaborators. Use when access denied, chmod, harbor, allocation, ACL.
---
# Permissions & access control

Layers: identity → **groups** → POSIX/VOSpace ACLs → Harbor.

**Groups (primary):** see `canfar-groups` for membership admin.

Docs: [Permissions](https://opencadc.github.io/canfar/latest/platform/permissions/)

## Project allocations

`/arc/projects/<name>` is a **managed allocation** (quota + team group):

- **Not** created with `mkdir`
- Request via allocations process / `support@canfar.net`
- PI/group admin ties allocation to CADC group

## POSIX on ARC

```bash
ls -la /arc/projects/mygroup/
chmod 664 shared.fits
chmod 755 scripts/run_pipeline.sh
chgrp mygroup shared.fits    # when group ownership needed
```

## VOSpace ACLs

Vault and ARC VOSpace: web Properties → Permissions, or `vchmod`.
Public release: explicit **other-read** on Vault — see `canfar-vospace`.

## Harbor (containers)

- **Pull** public project images anonymously
- **Push** requires project permissions on `images.canfar.net`
- Team images: grant group access to Harbor project

## API / automation

Same CADC identity for CLI, Python client, and archives.
Tokens/certs — `canfar-auth`.

## Ray / shared compute

Batch and headless jobs run as **your** identity — input paths on `/arc/projects`
must be readable by you (and group if workers share group context).

## Agent rules

1. Permission denied → **group membership** first, not `sudo`.
2. Public data → **Vault** with explicit ACLs; ARC projects stay private by default.
3. External partners → PI adds to **group**, not ad-hoc world-readable home dirs.

Related: `canfar-groups`, `canfar-auth`
