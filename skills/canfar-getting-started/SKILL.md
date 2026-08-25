---
name: canfar-getting-started
description: >
  Getting started on CANFAR: CADC or SRCNet account, request access, join a project
  via PI/group, Science Portal first session, fair use, acknowledgement text.
  Use for new users, access request, how to begin, who can use CANFAR, cost.
---
# Getting started on CANFAR

## Who can use CANFAR

- **Free** for astronomical research (fair-use and allocation limits apply)
- Canadian astronomers and collaborators; SRCNet partners on their nodes
- Larger needs: [Alliance Resource Allocation](https://docs.alliancecan.ca/)

## Deployment note

CANFAR is **open source** — sites run their own Science Portal (CADC, SRCNet, …).
Examples below use **CADC** (`www.canfar.net`). SRCNet users typically:

```bash
pip install canfar
canfar login srcnet
canfar auth show
```

## Access path (CADC)

### 1. CADC account

Request at: [CADC account request](https://www.cadc-ccda.hia-iha.nrc-cnrc.gc.ca/en/auth/request.html)

### 2. CANFAR platform access

**Option A — email** (typical 1–2 business days):

Email `support@canfar.net` with CADC username and brief research description.

**Option B — join existing team:**

Ask your PI to add you to the project's **CADC group** (see `canfar-groups`).

### 3. First session

1. Science Portal → log in (CADC: [canfar.net/science-portal](https://www.canfar.net/science-portal/))
2. Choose session type (Notebook, Desktop, Contributed, …)
3. Work under `/arc/projects/<group>/` or `/arc/home/<you>/`
4. Use `/scratch` for temp processing — **copy results to `/arc` before ending**

```bash
canfar login cadc
canfar create --name first-test notebook skaha/astroml:latest
canfar ps
```

## Key concepts (learn next)

| Topic | Skill |
| --- | --- |
| Storage tiers | `canfar-storage` |
| Teams & groups | `canfar-groups` |
| Session types | `canfar-sessions` |
| Auth / SRCNet | `canfar-auth` |

## Acknowledgement (papers/theses)

> The authors acknowledge the use of the Canadian Advanced Network for Astronomy Research (CANFAR) Science Platform operated by the Canadian Astronomy Data Centre (CADC) and the Digital Research Alliance of Canada…

Full text: [CANFAR home](https://opencadc.github.io/canfar/)

## Help

- [Getting started guide](https://opencadc.github.io/canfar/latest/platform/get-started/)
- [FAQ](https://opencadc.github.io/canfar/latest/platform/support/faq/)
- CADC: `support@canfar.net` · deployment Discord (see your portal)
