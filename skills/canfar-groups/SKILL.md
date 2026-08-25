---
name: canfar-groups
description: >
  CANFAR CADC groups: create research group, add members and administrators,
  search users by name, group-based access to /arc/projects, containers, and
  collaboration. Use when adding collaborator, postdoc, student, PI, team
  membership, group admin, or "can't access project directory".
---
# Groups & collaboration

Groups are the foundation of **team access** on CANFAR — shared project
storage, containers, and permissions flow through CADC group membership.

**Portal:** [CADC Group Management](https://www.cadc-ccda.hia-iha.nrc-cnrc.gc.ca/en/groups/)

## Roles

| Role | Can do |
| --- | --- |
| **Administrator** | Add/remove members, assign admins, manage group resources |
| **Member** | Use shared resources the group grants (e.g. `/arc/projects/…`) |

## Create a research group

1. Group Management portal → **New Group**
2. Descriptive name (e.g. `cfhtls-survey`, `exoplanet-collab`)
3. Project description → **Create**

## Add team members

1. **Edit** in the Membership column
2. Search by **full name** (e.g. "Jane Doe") — not always by username
3. Select user → **Add member**

## Assign administrators

1. **Edit** in the Administrators column
2. Add users who should manage membership and allocations

## Group → project storage

- `/arc/projects/<project>/` is tied to a **project allocation** + **team group**
- Members of the group get POSIX access to that path
- Creating a project allocation is **not** `mkdir` — admin/allocation workflow (see `canfar-permissions`)

## External collaborators

PI adds external partners to the group when policy allows; they use the same
CADC identity across CANFAR, archives, and VO services.

## Troubleshooting

| Problem | Check |
| --- | --- |
| Permission denied on `/arc/projects/foo` | User in project's group? PI adds via portal |
| New hire can't see data | Group membership, not separate CANFAR "account" per project |

Related: `canfar-permissions` (ACLs), `canfar-storage` (paths)
