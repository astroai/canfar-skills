---
name: canfar-cvmfs
description: >
  CVMFS on CANFAR: /cvmfs/soft.computecanada.ca Alliance software stack,
  environment modules module load, read-only lazy mount, vs containers and /arc
  installs. Use when accessing Alliance software, module load, or ls /cvmfs empty.
---
# CVMFS software

Docs: [CANFAR CVMFS guide](https://opencadc.github.io/canfar/latest/platform/cvmfs/)

## What it is

**CernVM File System** — read-only, distributed software trees. Maintained by
**Digital Research Alliance of Canada** on Alliance-backed clusters.

- **Not** writable — install custom packages under `/arc/home` or project envs
- **Not** a substitute for `/arc` project storage
- **Complements** containers: lean image + shared stack on demand

## Deployment note

CVMFS is **cluster infrastructure**, not configured in the Skaha helm chart.
When your deployment mounts it, sessions see Alliance software at:

```bash
source /cvmfs/soft.computecanada.ca/config/profile/bash.sh
module avail
module load python/3.11
module load gcc openmpi
which python
```

If `/cvmfs` is empty, CVMFS may not be enabled on your site — use container images or project envs instead.

## Lazy mount gotcha

`ls /cvmfs` may look **empty** — repos mount when you access a **known path**:

```bash
ls /cvmfs/soft.computecanada.ca/
```

Always start from documented paths; do not browse `/cvmfs` like `/usr`.

## vs containers

| Approach | When |
| --- | --- |
| **Container image** (Harbor `skaha/*`) | Reproducible stack baked in |
| **CVMFS modules** | Alliance-maintained HPC stack without huge images |
| **pixi/uv/conda on `/arc`** | Project-specific deps you control |

## AstroAI images

AstroAI ships stacks in `/opt/astroai/venv/*` and project **pixi/uv** under `$WORK`.
Use CVMFS when you need Alliance modules **not** in the image.

## Agent rules

1. Never `pip install` into `/cvmfs`.
2. CVMFS cache is **per K8s node** — cold start on a new node may be slower.
3. Batch jobs inherit CVMFS when the cluster mounts it.
