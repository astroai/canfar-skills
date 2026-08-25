---
name: canfar-python-client
description: >
  CANFAR Python client: pip install canfar, Session AsyncSession create connect
  events destroy, programmatic session management, automation scripts. Use when
  automating CANFAR from Python, CI pipelines, session API.
---
# Python client

Docs: [Python client](https://opencadc.github.io/canfar/latest/client/home/)

```bash
pip install canfar --upgrade
canfar login cadc
```

## Session (sync)

```python
from canfar.sessions import Session

session = Session()
ids = session.create(
    kind="notebook",
    image="images.canfar.net/skaha/astroml:latest",
    name="my-analysis",
)
session.connect(ids)
# ... later ...
session.destroy(ids)
```

## Headless / batch

```python
ids = session.create(
    kind="headless",
    image="images.canfar.net/skaha/astroml:latest",
    name="batch-reduce",
    cmd="python",
    args="/arc/projects/mygroup/scripts/reduce.py",
    cores=4,
    ram=16,
    env={"OMP_NUM_THREADS": "4"},
)
```

## AsyncSession

```python
from canfar.sessions import AsyncSession

async with AsyncSession() as session:
    ids = await session.create(
        kind="headless",
        image="images.canfar.net/skaha/astroml:latest",
        name="async-job",
        cmd="python",
        args="/arc/projects/demo/run.py",
    )
    await session.events(ids, verbose=True)
```

## Modules

| Module | Purpose |
| --- | --- |
| `canfar.sessions` | Create, connect, logs, destroy |
| Auth helpers | Noninteractive scripts — see `canfar-auth` |

More: [Examples](https://opencadc.github.io/canfar/latest/client/examples/) · [API reference](https://opencadc.github.io/canfar/latest/client/session/)

## Agent rules

1. Always `canfar login` (or saved creds) before scripts run.
2. Put **data paths on `/arc/projects`**, not scratch, for multi-step automation.
3. Prefer headless `kind` for CI; notebook for debugging.
