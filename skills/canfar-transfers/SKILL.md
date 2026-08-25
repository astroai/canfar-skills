---
name: canfar-transfers
description: >
  CANFAR data transfers: upload download between laptop and CANFAR, SSHFS mount,
  rsync, VOSpace vcp, direct curl URLs, web file manager, scratch to arc sync,
  large dataset movement. Use when moving files, upload FITS, sync data, SSHFS,
  rsync, transfer from local machine.
---
# Data transfers

Docs: [Data transfers](https://opencadc.github.io/canfar/latest/platform/storage/transfers/)

## Choose a method

| Size / scenario | Method |
| --- | --- |
| Small (<1 GB) | Web file manager or Vault UI (CADC: canfar.net/storage) |
| Medium (1–100 GB) | VOSpace CLI (`vcp`), direct HTTPS URLs |
| Large / sync | `rsync` over SSHFS, `vsync` |
| Inside session | `cp` between `/scratch` and `/arc` |

## Inside a session

```bash
cp /arc/projects/mygroup/raw/large.fits /scratch/
# ... process ...
cp /scratch/results.csv /arc/projects/mygroup/results/
```

## VOSpace CLI

```bash
canfar login   # or cadc-get-cert
vcp ./local.fits vos:myuser/incoming/
vcp vos:myuser/archive.fits /arc/projects/mygroup/data/
vsync /arc/projects/mygroup/out vos:myuser/published/
```

See `canfar-vospace` for Vault vs ARC URIs.

## `canfar data cp` (CLI)

Uses storage identifiers, not bare mount paths:

```bash
canfar data cp local:/absolute/path/file.fits arc:/projects/mygroup/file.fits
canfar data cp vault:/folder/file.fits arc:/home/$USER/file.fits
```

## Direct HTTPS (CADC example)

```bash
cadc-get-cert --user $USER
curl --cert ~/.ssl/cadcproxy.pem --upload-file file.fits \
  https://ws-uv.canfar.net/arc/files/projects/mygroup/file.fits
```

Host varies by deployment — see your site's filesystem access docs.

## SSHFS (from your laptop)

Mount `/arc/home` or project space — SSH keys in `~/.ssh/authorized_keys` on
CANFAR. See [Filesystem access](https://opencadc.github.io/canfar/latest/platform/storage/filesystem.md).

## Agent rules

1. Never leave the only copy on `/scratch` — session end deletes it.
2. Vault for **public/archive**; `/arc/projects` for **active team** work.
3. Match method to size — web UI fails gracefully on multi-TB; use CLI.
