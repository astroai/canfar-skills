---
name: canfar-cadc-data
description: >
  CADC astronomical archives on CANFAR: cadcget, cadcaccess, archive download,
  VO services, relationship to CANFAR storage. Use when downloading survey data,
  HST, CFHT, archive FITS, CADC catalog, not for VOSpace user files.
---
# CADC archive data

**CANFAR** = compute + team storage. **CADC archives** = observatory survey holdings.

Archives: [CADC](https://www.cadc-ccda.hia-iha.nrc-cnrc.gc.ca/)

## CLI (in CANFAR sessions)

Platform CLIs often include:

```bash
cadcget <archive-id> <file-id>   # download to cwd
# browse services at CADC web portal first
```

Exact commands depend on image — `which cadcget`, `cadcget --help`.

On **AstroAI** images: `/opt/astroai/venv/cadc` on PATH.

## Typical workflow

1. Discover data via CADC search portal
2. Download to **`/arc/projects/<group>/raw/`** (persistent, shared)
3. Process on **`/scratch`** inside sessions
4. Publish results → `/arc/projects/…/results/` or Vault (`canfar-doi`)

## vs VOSpace

| | CADC archives | Your VOSpace (Vault/ARC) |
| --- | --- | --- |
| Content | Curated surveys | Your team files |
| Access | Archive policies | Your groups/ACLs |
| Tool | `cadcget`, TAP, … | `vcp`, POSIX |

## Agent rules

1. Do not confuse archive IDs with `vos:` user paths.
2. Large downloads → target **`/arc/projects`**, not home quota.
3. Same **CADC identity** used for CANFAR login and archive access.

Related: `canfar-storage`, `canfar-transfers`
