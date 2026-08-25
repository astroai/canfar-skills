---
name: canfar-troubleshooting
description: >
  CANFAR troubleshooting: session Pending stuck, home quota full, scratch not
  visible to teammate, files lost after session end, canfar auth failed,
  Jupyter or hub not loading, permission denied, batch job failed. Use when
  something broke or behavior seems wrong on CANFAR.
---
# Troubleshooting

## Diagnostics bundle

```bash
canfar ps
canfar auth show
canfar stats
canfar events <session-id>
df -h /arc/home/$USER /scratch 2>/dev/null
du -sh /arc/home/$USER/* 2>/dev/null | sort -h | tail
```

On **AstroAI** images add:

```bash
astroai status --json
astroai env export
less /opt/astroai/USAGE.md
```

## Symptom → action

| Symptom | Likely cause | Action |
| --- | --- | --- |
| Teammate can't see `/scratch/...` | Scratch is session-private | `/arc/projects` or `vcp` / `canfar data cp` |
| Files gone after session delete | Never copied to `/arc` | Lost if only on scratch — persist next time |
| Home save/login fails | Home quota full | `df`/`du`; move data to project; `canfar-quotas` |
| **Create rejected** "maximum … sessions" | Interactive session cap hit | `canfar ps`; delete idle sessions |
| New session **Pending** long | Queue, image pull, probe, Kueue | `canfar events <id>`; not always "cap" |
| **Permission denied** on project | Not in group / allocation | `canfar-groups`; PI adds member |
| `canfar login` fails | Cert/IDP expired | `canfar-auth`; re-login |
| `vcp` / Vault slow or fails | Large file via web UI | `canfar-transfers`; CLI `vcp` |
| Batch job **Failed** | Bad image, OOM, bad path | Session logs; paths on `/arc`; `canfar-batch` |
| Quota % stuck after delete | Ceph `rbytes` lag | Wait; re-check `df` |
| CVMFS empty at `/cvmfs` | Lazy mount or not enabled | `ls /cvmfs/soft.computecanada.ca/` |
| Can't push Harbor image | No registry permission | `canfar-permissions`; project admin |

## Platform vs user error

1. **Scratch invisible to others** — by design, not a bug.
2. **Project dir missing** — allocations are managed; `mkdir` under `/arc/projects/foo` won't create a project.
3. **Archive download fails** — separate from VOSpace; see `canfar-cadc-data`.

## Escalation

Platform outages, persistent cluster-wide Pending:

- CADC: **`support@canfar.net`** · [Discord](https://discord.gg/vcCQ8QBvBa)
- Other deployments: your portal support / operator contact

## Agent rules

1. Confirm persistence tier before calling it a bug (scratch vs arc vs vault).
2. Give one concrete command + expected output.
3. Do not advise `sudo` — users lack root in sessions.
