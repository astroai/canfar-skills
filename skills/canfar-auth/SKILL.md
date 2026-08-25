---
name: canfar-auth
description: >
  CANFAR authentication: canfar login, CADC IDP, srcnet, science platform server
  selection, canfar auth show, certificates cadc-get-cert, noninteractive scripts.
  Use when login fails, auth, certificate, IDP, srcnet, credentials.
---
# Authentication

CANFAR separates **who you are** (IDP) from **which server** runs sessions.

Docs: [Authentication & servers](https://opencadc.github.io/canfar/latest/cli/authentication-contexts/)

## Deployment note

Built-in IDPs in the open-source client:

| IDP | Auth | Primary storage leaf |
| --- | --- | --- |
| `cadc` | X.509 certificate | `arc` + `vault` |
| `srcnet` | OIDC (SKA IAM) | `cavern` |

After login, check active server: `canfar auth show` · `canfar server ls`

## Interactive login

```bash
canfar login              # choose IDP + server
canfar login cadc
canfar login srcnet
canfar login cadc --force   # re-authenticate
```

Debug:

```bash
canfar --log-level debug login cadc --force
```

## Inspect / remove

```bash
canfar auth show
canfar auth ls
canfar auth rm cadc
canfar auth purge --force
```

No `canfar logout` command — use `auth rm` or `auth purge`.

Credentials persist under `/arc/home` (e.g. `~/.canfar/config.yaml`) when mounted.

## Certificates (VOSpace / legacy tools)

```bash
cadc-get-cert -u $USER    # ~10 days typical, ~/.ssl/cadcproxy.pem
```

Prefer `canfar login` for new workflows.

## Noninteractive / automation

1. Run `canfar login` once interactively on a machine with `/arc/home` access, or
2. Use Python client with stored auth — see [Client auth](https://opencadc.github.io/canfar/latest/client/get-started/)

## Multi-server / dev

| Flag | Use |
| --- | --- |
| `--dev` | Include development servers in discovery |
| `--timeout`, `-t` | Slow networks |

Switch server: `canfar server ls` · `canfar server use <name>`

## Agent rules

1. "Not authenticated" in batch jobs → worker must see valid creds on mounted home.
2. Do not embed passwords in scripts — use platform login + file-based auth.
3. `canfar auth login` is deprecated alias; use `canfar login`.

Related: `canfar-cli`, `canfar-python-client`
