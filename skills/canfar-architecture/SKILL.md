---
name: canfar-architecture
description: >
  CANFAR platform architecture: Science Portal, Skaha session manager,
  Kubernetes, Harbor registry, CADC authentication, ARC/Cavern storage, VOSpace
  vault, scratch SSDs, session types (notebook, contributed, headless, batch).
  Use when explaining how CANFAR works, AstroAI vs CANFAR naming, or what
  Skaha/Harbor/ARC mean.
---
# CANFAR architecture

## Names (AstroAI sessions)

| Name | What |
| --- | --- |
| **CANFAR** | Science Platform — portal, Skaha, `/arc`, auth |
| **Skaha** | Session manager (K8s jobs for notebooks, webterm, Ray, …) |
| **CADC** | Identity + archives (`canfar login`, `cadcget`) |
| **astroai** | In-session CLI — env, Ray, agents, `status` |
| **canfar** | Platform CLI — sessions, auth, `canfar data` |

## Component stack

```text
Scientist → Science Portal (canfar.net)
         → CADC auth
         → Skaha (session scheduler)
         → Kubernetes pods (container images from Harbor)
         → Storage mounts: /arc, /scratch, /cvmfs
         → VOSpace (vault + arc APIs) for object I/O
```

## Session types (Skaha)

| Type | Examples | Interactive |
| --- | --- | --- |
| **Notebook** | JupyterLab `:8888` | Yes |
| **Contributed** | webterm, vscode, marimo, openresearch, ray-manager `:5000` | Yes |
| **Headless** | ray-worker, batch, improc batch | No |
| **Batch** | Scheduled jobs | No |

AstroAI images live in Harbor project **`astroai`**
(`images.canfar.net/astroai/*`).

## Storage tiers (summary)

| Tier | Path | Backing |
| --- | --- | --- |
| Scratch | `/scratch` | Local SSD, session pod |
| ARC home | `/arc/home/<user>` | CephFS POSIX (~10 GB default) |
| ARC projects | `/arc/projects/<group>` | CephFS POSIX, team quota |
| Vault VOSpace | `vos:`, web UI | Object store, geo-redundant |
| CVMFS | `/cvmfs/soft.computecanada.ca` | Read-only software |

Scratch is **fast and private** to one session. ARC is **shared and
persistent**. Vault is for **long-term archive and public sharing**.

## AstroAI-specific layout

On CANFAR AstroAI images:

- `$WORK` = `$SCRATCH/src` — code and pixi/uv projects (survives container OOM)
- `$SCRATCH` — data, caches, agent CLIs (`ASTROAI_LAB_BIN_DIR`)
- `$HOME` = `/arc/home/<you>` — small persistent config only

## Agent rules

1. Point users to [opencadc.github.io/canfar](https://opencadc.github.io/canfar/) for platform-wide detail.
2. For AstroAI daily commands, `astroai-lab-workflow` skill complements this.
3. Do not conflate `astroai status` (this session) with `canfar ps` (all sessions).
