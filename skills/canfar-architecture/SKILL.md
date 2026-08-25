---
name: canfar-architecture
description: >
  CANFAR platform architecture: Science Portal, Skaha session manager,
  Kubernetes, Harbor registry, CADC authentication, group management, ARC/Cavern
  storage, VOSpace vault, scratch SSDs, batch and headless jobs. Use when
  explaining how CANFAR works, what Skaha/Harbor/ARC mean, or platform vs
  AstroAI tooling.
---
# CANFAR architecture

Docs: [Platform overview](https://opencadc.github.io/canfar/latest/)

## Core components

| Name | Role |
| --- | --- |
| **CANFAR** | Science Platform — portal, sessions, storage, auth |
| **Science Portal** | Web UI (hostname varies by deployment) |
| **Skaha** | Session scheduler (K8s jobs: notebook, desktop, batch, …) |
| **Harbor** | Container registry (default CADC: `images.canfar.net`) |
| **CADC** | Canadian Astronomy Data Centre — identity + archives |
| **Group management** | CADC Group Management Portal — team membership (`canfar-groups`) |
| **VOSpace** | Object storage API (Vault + ARC views) |

## Request flow

```text
User → Science Portal / canfar CLI
    → Authentication (canfar-auth) — CADC x509 or SRCNet OIDC
    → Skaha schedules K8s pod
    → Container from Harbor image (registryHosts deployer-configurable)
    → Mounts: /arc (or cavern), /scratch, optionally /cvmfs
    → VOSpace APIs for vos: URIs
```

## Session types (Skaha)

| Type | Examples | Interactive |
| --- | --- | --- |
| **Notebook** | JupyterLab | Yes |
| **Desktop** | Linux GUI, CASA | Yes |
| **CARTA / Firefly** | Domain viewers | Yes |
| **Contributed** | Community web apps | Yes |
| **Headless / Batch** | Parallel replicas | No |

Detail: `canfar-sessions`, `canfar-batch`.

## Storage tiers

| Tier | Path | Backing | Shared? |
| --- | --- | --- | --- |
| Scratch | `/scratch` | Pod-local SSD (emptyDir) | No (session) |
| ARC home | `/arc/home/<user>` | CephFS (~10 GB CADC default) | Your sessions |
| ARC projects | `/arc/projects/<group>` | CephFS, team quota | Group |
| Vault | `vos:`, web UI | Object store | ACL-based |
| CVMFS | `/cvmfs/soft.computecanada.ca` | Read-only software | When cluster mounts it |

Scratch = fast, ephemeral. ARC = POSIX collaboration. Vault = archive + public URLs.

## Tooling layers

| Tool | Scope |
| --- | --- |
| `canfar` | Platform: auth, sessions, data staging |
| `vcp` / `vls` | VOSpace I/O |
| `cadcget` / TAP | CADC **archives** (not your vos space) |
| **`astroai`** | **AstroAI images only** — env, agents, Ray |

Do not conflate `canfar ps` (all your sessions) with in-session monitors.

## AstroAI on CANFAR (optional)

Some sites ship AstroAI Harbor images (`images.canfar.net/astroai/*`):

- `$WORK` = `$SCRATCH/src` — project code (survives container OOM)
- `$SCRATCH` — per-session data and agent runtimes
- `$HOME` = `/arc/home/<you>` — small persistent config

See `canfar-concurrency`, `astroai-ray`.

## Agent rules

1. Official detail: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)
2. Collaboration = **groups + project allocations**, not shared scratch.
3. Archives (CFHT, Gemini, …) ≠ user VOSpace — route to `canfar-cadc-data`.
