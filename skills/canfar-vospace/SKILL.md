---
name: canfar-vospace
description: >
  CANFAR VOSpace: Vault long-term storage, ARC VOSpace API, vos URIs, vcp vls
  vsync vmkdir vchmod, web file managers, metadata sharing public URLs, Python vos.
  Use for vault, VOSpace, vos colon, archive sharing, publish data.
---
# VOSpace (Vault & ARC)

Docs: [VOSpace guide](https://opencadc.github.io/canfar/latest/platform/storage/vospace/)

## When to use which

| | **Vault** | **ARC** (home/projects) | **Scratch** |
| --- | --- | --- | --- |
| Purpose | Archive, share, publish | Active team work | Temp compute |
| Backup | Geo-redundant | Daily snapshots (CADC) | None |
| Speed | Slower | Medium (POSIX) | Fastest |
| Public URLs | Yes | No | No |
| Access | Web + `vos:` API | POSIX + VOSpace view | Session only |

**ARC** and **Cavern** = same VOSpace image backing POSIX mounts on many deployments.

## Web managers (CADC example)

- [Vault](https://www.canfar.net/storage/vault/list/)
- [ARC VOSpace view](https://www.canfar.net/storage/arc/list/)

Upload, permissions (right-click → Properties), public links.

## CLI

```bash
canfar login
vls vos:myuser
vls vos:myuser/projects/
vcp ./table.fits vos:myuser/releases/v1/
vcp vos:myuser/in/large.fits /scratch/
vsync /arc/projects/mygroup/out vos:myuser/published/
vmkdir vos:myuser/newdir
vchmod g+w vos:myuser/shared/   # when needed
```

Legacy cert path: `cadc-get-cert -u $USER` → `~/.ssl/cadcproxy.pem` (~10 days typical)

## Sharing model

| Permission | Meaning |
| --- | --- |
| Read (r) | List/download |
| Write (w) | Modify/delete |
| Execute (x) | Traverse directories |

Targets: **Owner**, **Group** (see `canfar-groups`), **Other** (public).

## ARC via VOSpace URI

```bash
vcp file.fits vos:/arc:projects/mygroup/incoming/
# or: canfar data cp … arc:/projects/mygroup/incoming/file.fits
```

POSIX path `/arc/projects/mygroup/` and VOSpace view of same allocation.

## Large transfers

Use `vcp`/`vsync`, not web UI — see `canfar-transfers`.

## Publication

DOI workflow uses Vault — `canfar-doi`.

## Agent rules

1. Vault for **citation-ready** releases; `/arc/projects` for **active** collaboration.
2. `vos:` paths work **across sessions** — good handoff between people.
3. SRCNet `canfar login srcnet` maps primary storage leaf **`cavern`**, not always `arc`.

Related: `canfar-cadc-data` (archives ≠ your vos space)
