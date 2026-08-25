---
name: canfar-cvmfs
description: >
  CVMFS on CANFAR: /cvmfs/soft.computecanada.ca Alliance software stack,
  environment modules module load, read-only lazy mount, vs containers and /arc
  installs. Use when accessing Alliance software, module load, or ls /cvmfs empty.
---
# CVMFS software

## What it is

**CernVM File System** — read-only, distributed software trees mounted in **all**
CANFAR sessions (notebook, desktop, batch). Maintained by **Digital Research
Alliance of Canada**.

- **Not** writable — install custom packages under `/arc/home` or project envs
- **Not** a substitute for `/arc` project storage
- **Complements** containers: small image + large shared stack on demand

## Path (CANFAR / Alliance)

```bash
source /cvmfs/soft.computecanada.ca/config/profile/bash.sh
module avail
module load python/3.11
module load gcc openmpi
which python
```

## Lazy mount gotcha

`ls /cvmfs` may look **empty** — repos mount when you access a **known path**:

```bash
ls /cvmfs/soft.computecanada.ca/
```

Always start from documented paths; do not browse `/cvmfs` like `/usr`.

## vs AstroAI images

AstroAI ships heavy stacks in **`/opt/astroai/venv/*`** and project **pixi/uv**
under `$WORK`. Use CVMFS when you need Alliance modules **not** in the image.

## Agent rules

1. Never `pip install` into `/cvmfs`.
2. CVMFS cache is **per K8s node** — cold start on a new node may be slower.
3. More: [CANFAR CVMFS guide](https://opencadc.github.io/canfar/latest/platform/cvmfs/)
