# CANFAR platform skills

Agent skills for the [CANFAR Science Platform](https://www.opencadc.org/canfar/) and
[AstroAI](https://github.com/astroai/canfar-lab) sessions. Users ask in plain
language; the router skill picks the right guide.

Works for **CADC**, **SRCNet**, and self-hosted open-source deployments — examples
often cite CADC URLs; use `canfar auth show` / `canfar server ls` for your site.

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

## Skills (23)

| Skill | Topic |
|-------|--------|
| `canfar-platform` | Intent router (start here) |
| `canfar-getting-started` | Account, first session, CADC/SRCNet |
| `canfar-architecture` | Portal, Skaha, K8s, storage tiers |
| `canfar-sessions` | Notebook, desktop, CARTA, Firefly, lifecycle |
| `canfar-storage` | Scratch, ARC home/projects, layout |
| `canfar-vospace` | Vault, `vos:`, `vcp`/`vls`, sharing |
| `canfar-transfers` | SSHFS, rsync, `canfar data cp`, large uploads |
| `canfar-quotas` | Home/project/scratch/vault quotas |
| `canfar-groups` | CADC groups, membership, teams |
| `canfar-permissions` | Allocations, ACLs, Harbor |
| `canfar-batch` | Headless sessions, replicas (max 512) |
| `canfar-containers` | Harbor, astroml, contributed port 5000 |
| `canfar-auth` | Login, CADC/SRCNet IDP, certificates |
| `canfar-cli` | `canfar login/create/ps`, automation |
| `canfar-python-client` | Python Session API |
| `canfar-doi` | Data publication, DOI |
| `canfar-cadc-data` | CADC archive download |
| `canfar-limits` | cgroups, CPU/RAM/GPU, session caps |
| `canfar-cvmfs` | Alliance software at `/cvmfs` (when mounted) |
| `canfar-best-practices` | Scale-out, batch-friendly code |
| `canfar-concurrency` | Shared `/arc/home`, locks |
| `canfar-troubleshooting` | Common failures |
| `astroai-ray` | Ray cluster via `astroai` (AstroAI images only) |

Official platform docs: [opencadc.github.io/canfar](https://opencadc.github.io/canfar/)

On an AstroAI session: `less /opt/astroai/USAGE.md`

## License

Apache-2.0 — see [LICENSE](LICENSE).
