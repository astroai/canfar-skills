---
name: canfar-troubleshooting
description: >
  CANFAR troubleshooting: session Pending stuck, home quota full, scratch not
  visible to teammate, files lost after session end, canfar auth failed, agent
  setup refused, Jupyter or hub not loading, ceph quota lag. Use when something
  broke or behavior seems wrong on CANFAR.
---
# Troubleshooting

## Diagnostics bundle

```bash
astroai status --json
canfar ps
canfar auth show
astroai env export
less /opt/astroai/USAGE.md
```

## Symptom → action

| Symptom | Likely cause | Action |
| --- | --- | --- |
| Teammate can't see `/scratch/...` | Scratch is session-private | Use `/arc/projects` or `canfar data` / `vcp` |
| Files gone after session delete | Never copied to `/arc` | Restore from backup if on arc; else lost — persist next time |
| `astroai save` failed | Home quota full | `astroai status`; `astroai clean`; move data off home |
| Agent setup refused | Home quota ≥98% | Free `/arc/home`; avoid `--force` unless urgent |
| New session **Pending** long | Contributed quota (~3) or Skaha flake | `canfar ps`; delete idle; retry; check portal events |
| Quota % stuck after delete | Ceph `rbytes` lag | Wait/re-run `astroai status` |
| `canfar` not found | Wrong image / local laptop | Launch AstroAI session image |
| Cluster jobs fail auth | Missing cert | `canfar login`; `cluster status` → `auth: ok` |
| OpenCode/agent JSON broken | Schema drift | `astroai agent verify --fix` (one session at a time) |
| Ray job can't see data | Path on scratch | Put I/O on `/arc/projects/...` |

## Platform notes

- **Headless Pending** (ray-worker): often platform scheduling — see containers OPERATORS docs
- **Notebook** stock Skaha may skip AstroAI startup unless platform override
- **Home quota** prefers Ceph xattrs — trust `astroai status` over naive `df /arc/home`

## Agent rules

1. Confirm persistence tier before declaring "bug" (scratch vs arc).
2. Give one concrete command + expected output, not generic "check the portal".
3. Escalate platform outages to CADC support / portal status — not fixable in-session.
