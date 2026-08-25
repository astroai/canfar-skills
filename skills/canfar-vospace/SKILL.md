---
name: canfar-vospace
description: >
  CANFAR VOSpace: Vault vs ARC VOSpace, vos URIs, vcp vls vsync vmkdir vchmod,
  cadc-get-cert, web file manager, canfar data, public sharing, metadata.
  Use for vault, VOSpace, vos colon paths, or moving data to long-term storage.
---
# VOSpace (Vault & ARC)

## Vault vs ARC vs scratch

| | Vault | ARC (home/projects) | Scratch |
| --- | --- | --- | --- |
| Persistence | Permanent | Permanent | Session only |
| Backup | Geo-redundant | Daily snapshots | None |
| Speed | Slower (network) | Medium (POSIX) | Fast (local SSD) |
| Public URLs | Yes | No | No |
| CLI | `vcp`, `vls`, … | `cp`, `rsync` | `cp` |

**ARC** and **Cavern** name the same VOSpace image for user POSIX mounts.

## Web UI

- [Vault file manager](https://www.canfar.net/storage/vault/list/)
- [ARC file manager](https://www.canfar.net/storage/arc/list/)

## CLI (pre-installed on AstroAI)

```bash
cadc-get-cert -u $USER          # or use canfar login flow
vls vos:myuser
vcp ./result.fits vos:myuser/out/
vcp vos:myuser/in/large.fits /scratch/
vsync /scratch/processed vos:myuser/processed/
vmkdir vos:myuser/newdir
```

Authentication: `canfar login` or cert under `/arc/home`.

## canfar data (platform archive I/O)

Use when staging/syncing between ARC paths (not a replacement for all `vcp`):

```bash
canfar data stage /arc/projects/mygroup/raw
canfar data sync /scratch/out /arc/projects/mygroup/out
```

There is **no** `astroai` VOSpace wrapper — use `vcp`/`vls` or `canfar data`.

## Sharing

Vault: right-click → Properties → Permissions (r/w/x for owner/group/other).

ARC projects: **group membership** + POSIX ACLs — see `canfar-permissions`.

## Agent rules

1. Large interactive work on `/scratch` or `/arc`; vault for **publish/archive**.
2. `vos:` paths are not session-local — good for cross-session handoff.
3. For FITS archive access also consider `cadcget` / `cadcaccess` (CADC archives).
