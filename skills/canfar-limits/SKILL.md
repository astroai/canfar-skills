---
name: canfar-limits
description: >
  CANFAR session resource limits: Kubernetes cgroup memory CPU, astroai status
  cgroup_mem_pct, scratch disk, GPU nvidia-smi, flexible vs fixed session
  sizing, contributed session count limits. Use when OOM, throttled, or how
  much CPU RAM GPU this session has.
---
# Session limits

## What to inspect

```bash
astroai status --json
# fields: cpu_pct, mem_pct, cgroup_mem_pct, scratch.used/free, gpu[]
```

| Signal | Meaning |
| --- | --- |
| `cgroup_mem_pct` | Pod memory limit (K8s cgroup v2 `/sys/fs/cgroup/memory.*`) |
| `mem_pct` | Host-visible RAM (may differ from cgroup cap) |
| `cpu_pct` | Derived from load vs `nproc` |
| `scratch` | Local SSD usage for `/scratch` mount |
| `gpu` | `nvidia-smi` when GPUs allocated |

## Session sizing (Skaha)

**Flexible (default):** burst within cluster capacity; faster schedule.

**Fixed:** `--cpu N --memory M` reserves resources; may wait in queue.

```bash
canfar create --cpu 4 --memory 16 contributed images.canfar.net/astroai/webterm:26.08
canfar info <session-id>    # when available
```

## Contributed session count

Platform often caps **~3 concurrent contributed** sessions per user — if new sessions stay **Pending**, check `canfar ps` and delete idle sessions.

## Scratch vs memory

- **OOM in Python** → reduce batch size or request more `--memory`
- **Disk full on `/scratch`** → move to `/arc/projects` or delete caches; unrelated to home quota
- **`$WORK` on `/scratch/src`** survives container restart/OOM but not session delete

## GPUs

Only when session/image requests GPU resources. Verify with `astroai status --json` → `gpu` array.

Set thread envs to match allocated CPUs in parallel jobs (`OMP_NUM_THREADS`, etc.).

## Agent rules

1. Distinguish **cgroup OOM** (pod limit) from **home quota full** (ARC).
2. Do not advise requesting 100 CPUs in one session — prefer many small Ray jobs (`canfar-ray`).
3. Ray manager: memory **≥8 GiB** recommended.
