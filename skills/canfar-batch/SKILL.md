---
name: canfar-batch
description: >
  CANFAR batch and headless sessions: canfar create headless, pass command after
  --, replicas REPLICA_ID, fixed CPU memory, environment variables, Python Session
  batch jobs. Use for non-interactive processing, parallel jobs, automation,
  production pipelines — not AstroAI Ray (see astroai-ray).
---
# Batch & headless processing

**Headless** sessions run a command and exit — no Notebook, Desktop, or browser UI.
Same containers and `/arc` mounts as interactive sessions.

Docs: [Batch processing](https://opencadc.github.io/canfar/latest/platform/sessions/batch/)

## CLI

```bash
canfar login cadc

# Flexible resources (default)
canfar create --name reduce headless skaha/astroml:latest \
  -- python /arc/projects/mygroup/scripts/reduce.py

# Fixed resources
canfar create --name sim --cpu 16 --memory 64 headless skaha/astroml:latest \
  -- python /arc/projects/mygroup/scripts/simulation.py

# Environment variables
canfar create --name omp-test --cpu 4 --env OMP_NUM_THREADS=4 \
  headless skaha/astroml:latest -- python /arc/projects/mygroup/run.py

# Parallel replicas (independent slices, client max 512)
canfar create --name study --replicas 10 headless skaha/astroml:latest \
  -- python /arc/projects/mygroup/analyze.py
```

Each replica gets `REPLICA_ID` and `REPLICA_COUNT` — use for deterministic splits.
See [Distributed helpers](https://opencadc.github.io/canfar/latest/client/helpers.md).

`canfar run` and `canfar launch` are aliases for `canfar create`.

Headless sessions **do not count** toward the interactive session cap. Stock helm
default lifetime: **14 days** (separate from interactive expiry).

## Python client

```python
from canfar.sessions import Session

session = Session()
ids = session.create(
    name="nightly-reduction",
    image="images.canfar.net/skaha/astroml:latest",
    kind="headless",
    cmd="python",
    args="/arc/projects/mygroup/pipelines/reduce.py",
    cores=8,
    ram=32,
    replicas=10,
)
print(ids)
```

Async: `AsyncSession` + `await session.events(ids, verbose=True)` — see `canfar-python-client`.

## Data paths

- **Input/output on `/arc/projects/…`** — workers and collaborators can read
- **`/scratch`** only inside that session's pod — not shared across batch jobs unless each job copies from `/arc`

## vs AstroAI Ray

| | CANFAR batch (headless) | AstroAI Ray (`astroai-ray`) |
| --- | --- | --- |
| Platform | Any CANFAR user | AstroAI images + `astroai cluster` |
| Model | One Skaha session per job/replica | Ray manager + workers |

## Best practices

See `canfar-best-practices` — prefer many small parallel jobs over one huge container.
