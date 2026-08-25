# CANFAR platform skills

Agent skills for the [CANFAR Science Platform](https://www.opencadc.org/canfar/) and
[AstroAI](https://github.com/astroai/canfar-lab) sessions. Students ask in plain
language; the router skill picks the right guide.

## Install

**AstroAI (recommended)** — auto on `astroai agent setup`, or:

```bash
astroai agent plugins install canfar-platform
astroai agent plugins update canfar-platform
```

**Manual** (Cursor / other agents):

```bash
npx skills add astroai/canfar-skills
```

## Skills

| Skill | Topic |
|-------|--------|
| `canfar-platform` | Intent router (start here) |
| `canfar-architecture` | Portal, Skaha, K8s, storage tiers |
| `canfar-sessions` | Session types, lifecycle, AstroAI images |
| `canfar-storage` | Scratch, ARC home/projects, `$WORK` |
| `canfar-vospace` | Vault, `vos:`, `vcp`/`vls`, `canfar data` |
| `canfar-quotas` | Home/project/vault quotas, `astroai status` |
| `canfar-permissions` | Groups, ACLs, project allocations |
| `canfar-limits` | cgroups, CPU/RAM/GPU, scratch |
| `canfar-cvmfs` | Alliance software at `/cvmfs` |
| `canfar-concurrency` | Shared `/arc/home`, agent locks |
| `canfar-cli` | `canfar login/create/ps`, auth |
| `canfar-troubleshooting` | Common failures |
| `canfar-ray` | Ray cluster and jobs via `astroai` |

Official platform docs: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)

In an AstroAI session: `less /opt/astroai/USAGE.md`

## License

Apache-2.0 — see [LICENSE](LICENSE).
