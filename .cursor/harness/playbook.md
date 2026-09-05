# canfar-skills harness playbook

- `source-code-first`: Treat current OpenCADC implementation code and tests as
  behavioral truth. Public documentation is orientation and may lag.
- `deployment-values-live`: Use Helm templates for configurable behavior, then
  prefer deployed values and live service output for endpoints, mounts, limits,
  and enabled features.
- `audience-first`: Lead with Portal guidance for simple users; add reproducibility,
  collaboration, or automation details for scientists, teams, and power users.
- `paths-are-site-specific`: Label `/arc` examples as CADC-specific. Generic Skaha
  deployments may use `/cavern` or another configured mount.
- `core-vs-extension`: Distinguish OpenCADC core capabilities from optional site
  infrastructure. This repo documents the platform, not site-specific product CLIs.
- `smallest-verifier`: Run `python3 scripts/validate_skills.py` after skill edits.
