# CANFAR platform skills

Agent skills for the [CANFAR Science Platform](https://www.opencadc.org/canfar/) and
[AstroAI](https://github.com/astroai/canfar-lab) sessions. Users ask in plain
language; the router skill picks the right guide.

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

AstroAI agent plugins do **not** install skills — they configure MCP servers,
CLI tools, and Cursor rules only. Pair optional packs as needed:

```bash
npx skills add DietrichGebert/ponytail
astroai agent plugins install ponytail-rule

npx skills add probabl-ai/skills
astroai agent plugins install skore-cli
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
| `astroai-ray` | Ray cluster via `astroai` (AstroAI images only) |
| `astroai-lab-workflow` | Session workflow: pixi/uv, save/resume, agents |
| `ml-experimentation` | ML intent router (pairs with `probabl-ai/skills`) |

Public platform guide (check behavior against current code):
[www.opencadc.org/canfar](https://www.opencadc.org/canfar/latest/)

On an AstroAI session: `less /opt/astroai/USAGE.md`

## License

Apache-2.0 — see [LICENSE](LICENSE).
