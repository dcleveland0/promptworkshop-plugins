---
name: workshop-repo
description: Harden a task with the current repository attached. Use when the user explicitly invokes $workshop-repo or requests repo-aware PromptWorkshop context.
---

# Workshop Repo

Read `../promptworkshop/SKILL.md` completely, then follow it with these requirements:

1. Preserve the inline task and resolve the user's actual project `cwd` plus git remote before calling PromptWorkshop.
2. Call `promptworkshop_list_repo_packages` when available and name any saved build bound to the current repo.
3. Call native `promptworkshop_optimize_start` exactly once with `cwd`, `include_repo_context: true`, and the requested `repo_scan_level` (`default`, `large`, or `max`). Pass a matched `package_slug` when one exists, then use the same bounded `promptworkshop_optimize_wait` loop required by the parent skill.
4. If no saved build matches, continue with repo context and clearly report `Build: account default`; do not claim a repo build was applied.
5. Publish **Build prompt**, **Structure harness**, **Harden workflow**, and **Final QA**, show the hardened prompt, and wait for explicit approval.
6. If either durable MCP tool is unavailable, report the registration defect and stop. Never use a wrapper or REST fallback.

Match the workspace to a build in tiers, stopping at the first tier with exactly one candidate: exact normalized repo URL (strip a trailing `.git`, treat `git@host:org/repo` and `https://host/org/repo` as one identity, compare case-insensitively); then any search hint the user typed before `--`, matched case-insensitively against slug, name, and bound URL; then the same `org/repo` path on a different host; then the same repo name under a different owner. Prefer `promptworkshop_list_repo_packages` for the candidate list. Say which tier matched, and label a path or name match as a guess. If a tier yields more than one candidate, stop and show the shortlist rather than picking silently. If nothing matches, show the detected repo URL and the repo-bound builds that do exist. Depth comes from the shared token grammar; do not set `runRedTeam` here.

Progress tracking is mandatory: immediately after start returns, visibly report the `run_id`, then relay every changed `status_report.current_stage`, `percent`, and `message` between bounded wait calls. MCP notifications, tool spinners, and hidden tool activity are not a substitute for a visible assistant update.
