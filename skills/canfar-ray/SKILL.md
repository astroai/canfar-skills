---
name: canfar-ray
description: >-
  Drive a CANFAR Ray cluster and run jobs on it with astroai:
  cluster start/status/stop, cluster dashboard, and run/jobs submit/list/logs.
  Use when the user wants batch compute, Ray workers, or to run a script on
  an existing Ray cluster.
---

# CANFAR Ray with `astroai`

One CLI. Installed on AstroAI session images.

```bash
astroai --help
```

Usual path: one autoscaling manager, then a job with `--cpus`.

```bash
astroai cluster start
export ASTROAI_RAY_JOBS_ADDRESS=…    # skip inside the manager
astroai run train.py --cpus 2
```

`cluster start` writes `~/.config/canfar/lab/ray-manager.env`, creates the
manager if needed, and lets Ray add `ray-as-*` workers when the job needs
CPUs. Same as AstroAI hub **Start batch compute**.

Do not call `ray job submit`. The job command is `astroai run`.

## Start the cluster

```bash
astroai cluster start
astroai cluster start --max-workers 8 --cores 2 --ram 8
astroai cluster start --min-workers 1 --gpus 1 --timeout 1800
```

Prints `export ASTROAI_RAY_JOBS_ADDRESS=…`. The caller must export it in
their shell (a CLI cannot export into the parent). `--json` returns
`manager_url`, `jobs_address`, `dashboard_url`, `cluster_phase`,
`joined_workers`, and `autoscaling`.

If a manager was already running, `restart_manager` is true: stop it and
re-run `cluster start` so the new manager sources the env file.

`start` is safe to call again. It does not create a second manager.

## Run a job

```bash
export ASTROAI_RAY_JOBS_ADDRESS=…    # from start; skip inside the manager
astroai run train.py --cpus 2
astroai jobs submit --cmd 'python -m mosaic.stack --in /arc/projects/g/in' --wait
astroai jobs list
astroai jobs logs <run-id>
```

`--input` / `--output` URIs are stored on the Ray job. They are not copied.
Put data on `/arc`. `/scratch` dies with the session.

## Check, stop, dashboard

```bash
astroai cluster status
astroai cluster stop       # destroys workers AND the manager
astroai cluster dashboard           # Ray Dashboard URL (jobs, nodes, logs)
astroai cluster dashboard iframe    # notebook / marimo
```

`joined: N / M` is the health number. `auth: ok` means CANFAR credentials
are present.

`astroai status` is session CPU/disk/quota, not the cluster. Use
`cluster status` for the cluster.

## Rules for agents

1. The cluster autoscales: size it with `--max-workers`, never by launching
   workers yourself.
2. Prefer `--json` when you will parse. Plain text when showing the user.
3. `start` is safe to call again. It does not create a second manager.
4. After `start`, jobs are `astroai run` (or `astroai jobs submit --cmd`).
5. Workers cost money. Idle autoscaled workers stop on their own. Offer
   `cluster stop` when the user is done.
6. `start` already waits. If join is slow, give the user
   `cluster dashboard` instead of polling forever.
7. MCP `job_*` tools need Ray. Cluster tools do not. Same CLI functions
   either way.
8. Do not tell the user to write `ray-manager.env` by hand. `cluster start`
   and the hub button do that.
