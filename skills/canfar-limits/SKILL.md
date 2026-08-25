---
name: canfar-limits
description: >
  CANFAR session resource limits: Kubernetes cgroup memory CPU, nproc, scratch
  disk, GPU nvidia-smi, flexible vs fixed session sizing, per-user interactive
  session cap, astroai status on AstroAI images. Use when OOM, throttled, or how
  much CPU RAM GPU this session has.
---
# Session limits

## Inspect resources (any session)

```bash
nproc
free -h
df -h /scratch
cat /sys/fs/cgroup/memory.max 2>/dev/null || cat /sys/fs/cgroup/memory/memory.limit_in_bytes 2>/dev/null
nvidia-smi   # when GPU allocated
canfar stats   # cluster capacity
```

Portal session details and `canfar ps` show requested CPU/RAM/GPU.

## Deployer defaults (stock science-platform helm)

| Limit | Default | Notes |
| --- | --- | --- |
| Interactive session lifetime | **4 days** (`expirySeconds: 345600`) | CADC FAQ may cite 7 days — site-specific |
| Headless session lifetime | **14 days** | Hardcoded in headless launch template |
| Interactive sessions per user | **5** | Notebook+desktop+CARTA+Firefly+contributed combined |
| Scratch (`/scratch`) | **200Gi** ephemeral | **Desktop: 10Gi** only |
| Flexible CPU/RAM ceiling | up to **8 CPU / 32 GiB** | From Skaha context when LimitRange disabled |

Headless/batch jobs do **not** count toward the interactive session cap.

## AstroAI images (optional)

```bash
astroai status --json
# cpu_pct, mem_pct, cgroup_mem_pct, scratch.used/free, gpu[]
```

## Session sizing (Skaha)

| Mode | When |
| --- | --- |
| **Flexible** (default) | Exploration; burst within cluster |
| **Fixed** (`--cpu`, `--memory`) | Predictable performance; may queue |

```bash
canfar create --cpu 4 --memory 16 notebook skaha/astroml:latest
```

## OOM vs disk full

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Process killed, exit 137 | cgroup memory | Smaller batches; request `--memory` |
| `No space left` on `/scratch` | Scratch quota | Delete temp; move to `/arc/projects` |
| Save/login fails | **Home** quota | `canfar-quotas` |

Scratch full ≠ home full. Desktop sessions have much smaller scratch caps.

## GPUs

Request GPU in session config; use CUDA-enabled images. Set `OMP_NUM_THREADS` ≈ allocated CPUs for parallel jobs.

## Batch / Ray

Headless replicas each get their own cgroup. Client max **512 replicas** per create call.

**AstroAI Ray:** manager ≥8 GiB RAM recommended — `astroai-ray`.

## Agent rules

1. Distinguish cgroup OOM from ARC home quota.
2. Do not advise 100 CPUs in one interactive session — use batch replicas or Ray.
3. `/scratch` survives container restart but **not** session delete.
