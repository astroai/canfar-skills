# CANFAR platform skills

Agent skills for the [CANFAR Science Platform](https://www.opencadc.org/canfar/).
Users ask in plain language; the router skill picks the right guide.

The skills are written for students, scientists, research teams, and power users.
They start with the browser workflow when that is the simplest route, then add CLI
or Python API guidance when automation or scale makes it useful.

Guidance is grounded in the OpenCADC implementation: the `canfar` client and API,
Skaha and Science Portal, Cavern/VOSpace, CADC tools, and the deployment Helm
charts. Chart defaults are examples, not promises about a live site. Examples
often cite CADC; use `canfar auth show`, `canfar server ls`, and the Science Portal
to discover what a CADC, SRCNet, or compatible Skaha deployment actually exposes.

## Install

```bash
npx skills add astroai/canfar-skills
```

## Skills

| Skill | Topic |
|-------|--------|
| `canfar-platform` | Intent router (start here) |
| `canfar-getting-started` | Account, first session, CADC/SRCNet |
| `canfar-architecture` | Portal, Skaha, K8s, storage tiers |
| `canfar-sessions` | Notebook, desktop, CARTA, Firefly, lifecycle |
| `canfar-storage` | Scratch and persistent home/project storage |
| `canfar-vospace` | Vault, `vos:`, `vcp`/`vls`, sharing |
| `canfar-transfers` | SSHFS, rsync, `canfar data cp`, large uploads |
| `canfar-quotas` | Home/project/scratch/vault quotas |
| `canfar-groups` | CADC groups, membership, teams |
| `canfar-permissions` | Allocations, ACLs, Harbor |
| `canfar-batch` | Headless sessions, replicas (max 512) |
| `canfar-containers` | Harbor, astroml, contributed port 5000 |
| `canfar-auth` | Login, CADC/SRCNet IDP, certificates |
| `canfar-cli` | `canfar login/create/ps`, automation |
| `canfar-python-client` | Python Session, discovery, and storage APIs |
| `canfar-doi` | Data publication, DOI |
| `canfar-cadc-data` | CADC archive download |
| `canfar-limits` | cgroups, CPU/RAM/GPU, session caps |
| `canfar-cvmfs` | Alliance software at `/cvmfs` (when mounted) |
| `canfar-best-practices` | Scale-out, batch-friendly code |
| `canfar-concurrency` | Shared persistent homes, locks |
| `canfar-troubleshooting` | Common failures |

Public platform guide (check behavior against current code):
[www.opencadc.org/canfar](https://www.opencadc.org/canfar/latest/)

## License

Apache-2.0 — see [LICENSE](LICENSE).
