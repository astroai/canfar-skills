---
name: canfar-platform
description: >
  CANFAR Science Platform (CADC, SRCNet, or self-hosted). Use for any CANFAR
  question: account access, groups, sessions, scratch vs ARC vs Vault, quotas,
  transfers, permissions, containers, Harbor, Python client, CLI auth, DOI,
  CADC archives, CVMFS, limits, troubleshooting. Users do not name skills —
  read the matching skill below.
---
# CANFAR platform (intent router)

Users describe goals in plain language. **Do not ask them to pick a skill name.**
Read the skill that matches (under `~/.cursor/skills/` when `canfar-platform`
is installed).

## Deployment note

Examples use the **CADC** deployment (`www.canfar.net`, `images.canfar.net`).
Your site may differ — after login:

```bash
canfar auth show
canfar server ls
```

- **SRCNet:** `canfar login srcnet` (primary storage leaf `cavern`, not always `arc`)
- **Limits:** deployer-configured — check `canfar ps`, `canfar stats`, `canfar events <id>`
- **Support:** CADC uses `support@canfar.net`; other deployments see portal/Discord

## Route by intent

| User is trying to… | Read skill |
| --- | --- |
| Get account, first steps, acknowledgement | `canfar-getting-started` |
| Platform overview (Portal, Skaha, K8s, storage) | `canfar-architecture` |
| Launch/manage sessions, CARTA, Firefly, Desktop | `canfar-sessions` |
| Scratch vs `/arc/home` vs `/arc/projects` | `canfar-storage` |
| Vault, `vos:`, VOSpace sharing, public data | `canfar-vospace` |
| Move data (SSHFS, rsync, large uploads) | `canfar-transfers` |
| Quotas, disk full, request more space | `canfar-quotas` |
| Create/manage groups, add members | `canfar-groups` |
| ACLs, project allocations, Harbor access | `canfar-permissions` |
| Headless batch, replicas, parallel jobs | `canfar-batch` |
| Docker images, astroml, custom containers | `canfar-containers` |
| `canfar login`, IDP, certificates, servers | `canfar-auth` |
| `canfar create/ps`, CLI automation | `canfar-cli` |
| Python `Session` / `AsyncSession` | `canfar-python-client` |
| Publish data with DOI (DPS) | `canfar-doi` |
| CADC archive download (`cadcget`, etc.) | `canfar-cadc-data` |
| Session CPU/RAM/GPU, cgroup, scratch size | `canfar-limits` |
| Alliance software, `/cvmfs`, `module load` | `canfar-cvmfs` |
| Scale-out pipelines, batch-friendly code | `canfar-best-practices` |
| Shared `/arc/home`, concurrent sessions | `canfar-concurrency` |
| Failures, pending sessions, lost files | `canfar-troubleshooting` |
| **AstroAI only:** Ray cluster, `astroai run` | `astroai-ray` |

If storage + quota both apply: `canfar-storage` then `canfar-quotas`.

## Quick truths

| Path | Lifetime | Shared across sessions? |
| --- | --- | --- |
| `/scratch` | Session pod | **No** |
| `/arc/home/<you>` | Persistent (~10 GB default on CADC) | **Yes** |
| `/arc/projects/<group>` | Persistent (project quota) | **Yes** (group) |
| Vault / `vos:` | Persistent, geo-redundant | Per VOSpace ACLs |

```bash
canfar ps
canfar auth show
df -h /arc/home/$USER /arc/projects/<group> 2>/dev/null
```

Official docs: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)

On **AstroAI** images also: `astroai status --json` · `less /opt/astroai/USAGE.md`

## Install / update

```bash
astroai agent plugins install canfar-platform   # default on AstroAI setup
astroai agent plugins update canfar-platform
```
