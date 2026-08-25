---
name: canfar-platform
description: >
  CANFAR Science Platform and AstroAI sessions. Use when the user asks about
  CANFAR, CADC, Skaha, Science Portal, where to save files, scratch vs arc vs
  vault, home quota, project space, VOSpace, session limits, cgroup memory,
  permissions, groups, CVMFS software, canfar login or create, why a session is
  pending, shared home between sessions, or plain language: where do my files
  go, will my teammate see this, how much disk left, vault vs projects,
  session died, quota full. Students do not name skills — read the matching
  skill below.
---
# CANFAR platform (intent router)

**Do not ask students to pick a skill name.** Read the skill that matches
their question (under `~/.cursor/skills/` when `canfar-platform` is installed).

## Route by intent

| User is trying to… | Read skill |
| --- | --- |
| How CANFAR fits together (Portal, Skaha, K8s, storage tiers) | `canfar-architecture` |
| Start/stop sessions, images, flexible vs fixed resources | `canfar-sessions` |
| Scratch vs `$WORK` vs `/arc/home` vs `/arc/projects` | `canfar-storage` |
| Vault, `vos:`, `vcp`/`vls`, long-term object storage | `canfar-vospace` |
| Home/project/vault quota %, `astroai status` | `canfar-quotas` |
| Groups, ACLs, project allocations | `canfar-permissions` |
| Session CPU/RAM/GPU, cgroup, scratch size | `canfar-limits` |
| Alliance software, `module load`, `/cvmfs` | `canfar-cvmfs` |
| Two sessions, shared `/arc/home`, agent locks | `canfar-concurrency` |
| `canfar login`, `create`, `ps`, `canfar data` | `canfar-cli` |
| Pending session, lost files, quota full, scratch invisible | `canfar-troubleshooting` |
| Ray cluster, batch jobs, `astroai run` | `canfar-ray` |

If storage + quota both apply: read `canfar-storage` then `canfar-quotas`.

## Quick truths (memorize)

| Path | Lifetime | Shared across sessions? |
| --- | --- | --- |
| `$SCRATCH`, `$WORK` (`/scratch/src` on CANFAR) | Session | **No** — scratch is per-pod |
| `/arc/home/<you>` | Persistent | **Yes** |
| `/arc/projects/<group>` | Persistent | **Yes** (group ACLs) |
| Vault / `vos:` | Persistent | Per VOSpace permissions |

```bash
astroai status --json    # quotas, sessions, paths
canfar ps                # your Skaha sessions
less /opt/astroai/USAGE.md
```

Official docs: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)

## Missing skills?

On AstroAI: `astroai agent plugins install canfar-platform` (default on setup).
