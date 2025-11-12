Operations (minimal, no automation)
Source of truth: GitHub (Commits/PRs). No multi‑agent automation.
Units: 1–2 files per commit; ≤5 minutes per step.
Fallback: if a step blocks >5 min — move to the next step; return later.
Optional pulse (later): a GitHub Actions cron can append TICKs to automation/checks/heartbeat.md.
Logging: keep short notes in commits; heartbeat.md optional.
Roles (current): you commit manually; Deep provides small, atomic tasks and content.
Commit message:
docs: add minimal operations guide
