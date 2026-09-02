---
name: astroai-lab-workflow
description: >-
  AstroAI session workflow: pixi/uv under $WORK, astroai save/resume,
  Ray cluster start/run, agents. Use for new users or "how do I work here".
---
# AstroAI session in a few commands

**Names:** `canfar` = platform sessions/auth; `astroai` = in-session CLI
(env, Ray jobs, agents). AstroAI = product; CANFAR = host platform.

```bash
astroai agent setup              # once per user — MCP + Cursor rules
npx skills add astroai/canfar-skills   # platform + workflow + Ray skills
astroai agent install codex      # public GitHub release — no gh login needed
astroai agent install kilo       # or: goose, cline, opencode, cursor, …
gh auth login                    # only for GitHub MCP / private repos / git push
```

Discover what’s available: `astroai agent list` · plugins: `astroai agent plugins list`  
Plugins are MCP / CLI tools / Cursor rules only — not skills. Skills: `npx skills …`  
Optional lean ladder: `npx skills add DietrichGebert/ponytail` then `astroai agent plugins install ponytail-rule`  
Refresh after upgrading lab in-session: `astroai agent update`  
Broken agent configs (esp. OpenCode JSON): `astroai agent verify` · `astroai agent verify --fix`

## Daily workflow

```bash
astroai init mylab                # or astroai clone owner/repo
astroai clone --from-env ml-base owner/repo   # warm caches from saved stack
cd "${WORK}/mylab"
pixi install                     # or uv sync
pixi run python analysis.py
astroai save

astroai cluster start
astroai run train.py --cpus 2
astroai cluster status
```

Global flags (`--json`, `--yes`, `--dry-run`) work **before or after** the subcommand:
`astroai status --json`, `astroai save --list --json`.

## Storage (memorize this)

| Path | What |
|------|------|
| `${WORK}` | Code + project `.pixi`/`.venv` — ephemeral (on CANFAR: `$SCRATCH/src`, survives container OOM) |
| `${SCRATCH}` | Data, download caches, runtime installs (`ASTROAI_LAB_BIN_DIR`, uv/pixi roots) |
| `/opt/astroai/venv/cadc` | Platform CLIs: `canfar`, `cadcget`, `astroai` — **writable this session** |
| `/arc/projects/<team>/.local` | Shared team tools (persistent) |
| `/arc` (`$HOME`) | **Small only** — agent MCP config, gh auth, lockfile saves (`~/.astroai/lab`) |

**Project deps:** use pixi/uv lockfiles under `${WORK}` — that is where versions belong.
**Platform CLIs:** image installs are unpinned; bump in-session with `upgrade-cadc-tools.sh` (lost when the session ends).

```bash
upgrade-cadc-tools.sh list
upgrade-cadc-tools.sh --upgrade astroai-lab
astroai status --json
```

Avoid pip/uv/pixi/conda/npm **project** installs under `$HOME` — use project envs in `${WORK}` or team paths on `/arc/projects`.

Optional: `${WORK}/.astroai-lab/pythonpath` or `ASTROAI_LAB_PYTHONPATH` for extra import paths.

## Search & run (standard tools — no custom commands)

```bash
rg 'pattern' --type py
fd name
sg -p 'class $N' -l py          # needs: astroai agent plugins install ast-grep-cli
pixi run pytest -q
uv run python script.py
peek README.md                  # markdown/text; peek archive.tgz [member]
```

When showing the user a generated markdown, log, or archive in webterm (or any AstroAI session), prefer `peek <path>` (or `bat`/`less`) over dumping huge files raw.

## Help

```bash
astroai help
astroai cluster status
astroai status --json          # quotas, team projects, canfar auth/ps
astroai save --list --json
astroai agent list
less /opt/astroai/USAGE.md
```
