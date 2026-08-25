---
name: canfar-containers
description: >
  CANFAR containers and Harbor registry: astroml, casa, skaha images,
  images.canfar.net, custom Docker builds, contributed web apps port 5000,
  session image selection, reproducible environments. Use when picking container,
  custom image, Harbor, Dockerfile, contributed application.
---
# Containers & Harbor

Docs: [Containers](https://opencadc.github.io/canfar/latest/platform/containers/)

## Concepts

- **Container** = reproducible OS + astronomy stack (Python, CASA, …)
- **Runtime** = CANFAR mounts `/arc`, `/scratch`, `/cvmfs` (when enabled), allocates CPU/RAM
- Build once → same environment for you, team, and batch jobs

## Default platform images

Registry (CADC default): [images.canfar.net](https://images.canfar.net) — deployer sets `registryHosts`.

| Image | Use |
| --- | --- |
| `skaha/astroml:latest` | General Python / AstroPy stack |
| `skaha/casa:*` | CASA radio astronomy |
| `skaha/notebook:*` | Jupyter-focused |
| Community contributed | Portal → Contributed dropdown |

```bash
canfar create notebook skaha/astroml:latest
canfar create --name casa desktop skaha/casa:6.5
```

Two-part image names expand to `images.canfar.net/<name>` in the client.

## AstroAI contributed images (optional)

Harbor project `astroai`: `images.canfar.net/astroai/webterm`, `openresearch`, etc.
See `canfar-sessions` — only when your site runs AstroAI catalog.

## Custom images

1. [Container build guide](https://opencadc.github.io/canfar/latest/platform/containers/build.md)
2. Push to Harbor (project permissions via `canfar-permissions`)
3. Launch with full image URI:

```bash
canfar create contributed images.canfar.net/mygroup/myapp:1.0
```

## Contributed web applications

Skaha/Helm contract for contributed sessions:

- Web UI listening on **port 5000** (readiness probe)
- Image ENTRYPOINT/CMD starts the app — no `/skaha/startup.sh` required by server
- `/skaha/startup.sh` is a **desktop-app** image convention, not enforced for contributed

Contact your deployment support before large custom-app efforts.

## Agent rules

1. Prefer **published tags** (`:latest` or version) over `:dev` in production.
2. Pin image in papers/scripts for reproducibility.
3. Heavy deps can live in **CVMFS** (`canfar-cvmfs`) when cluster mounts it.
