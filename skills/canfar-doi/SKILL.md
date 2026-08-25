---
name: canfar-doi
description: >
  CANFAR Data Publication Service DOI: request DOI, upload data package, referee
  read-only access, DataCite publish, landing page, lock data directory. Use when
  publishing research data, DOI, data citation, DPS, peer review data access.
---
# Data Publication (DOI)

**DPS** links papers to permanent data packages with DataCite DOIs.

Docs: [DOI service](https://opencadc.github.io/canfar/latest/platform/doi/)

Portal: [Data Publication](https://www.canfar.net/citation/)

## Workflow

1. **Request DOI** — reserve identifier + Vault data directory + landing page draft
2. **Upload package** — web UI (small) or `vcp`/`vsync` (large) — see `canfar-vospace`
3. **Referee access** (optional) — CADC creates read-only reviewer accounts
4. **Publish** — registers with DataCite; **locks directory** (read-only forever)

!!! After publish, metadata/data changes need `support@canfar.net`.

## Upload tips

| Size | Method |
| --- | --- |
| Few small files | DPS web upload |
| Large / many files | `vcp`, `vsync` to assigned Vault path |

Organize with README, schema, calibration notes — see DPS guidelines in official doc.

## Requirements

- First author typically needs **CADC account**
- Data lives in **Vault VOSpace** (not scratch)

## Citation

Landing page URL + DOI (e.g. `10.11570/20.0006`) for paper data availability section.

Related: `canfar-vospace`, `canfar-transfers`
