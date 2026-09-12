---
name: workshop-auto
description: Harden a task with PromptWorkshop and execute it immediately. Use only when the user explicitly invokes $workshop-auto or requests auto-execution before the run.
---

# Workshop Auto

Read `../promptworkshop/SKILL.md` completely, then follow it with these overrides:

1. Treat explicit `$workshop-auto` invocation as the user's pre-run opt-out from PromptWorkshop review.
2. Parse an optional `--build <package_slug>` flag before `--`; preserve the task after `--` exactly. Validate an explicit build with `promptworkshop_list_packages` before model spend. There is no depth flag here — see 3.
3. Call native `promptworkshop_optimize_start` exactly once with `handoff_mode: "auto_execute"`, `workflow_tier: "full"`, the optional `package_slug`, and the current project `cwd`; then use the same bounded `promptworkshop_optimize_wait` loop required by the parent skill. Auto-execution is ALWAYS depth 3, harness and red-team included: nobody reads the hardened prompt before the work happens, so the red-team pass is the only check left and it is not optional. The server floors this regardless of what is sent. Never infer a tier from a slug.
4. Publish **Build prompt**, **Structure harness**, **Harden workflow**, and **Final QA** and verify provider-backed output.
5. Do not stop at the PromptWorkshop review gate. Execute the hardened task immediately and verify the resulting work.
6. Make the boundary explicit: report `PromptWorkshop complete; downstream execution started` before beginning the coding-agent work.
7. Do not bypass host permissions, destructive-action approval, credentials, billing, or workspace policy.
8. If either durable MCP tool is unavailable, report the registration defect and stop. Never use a wrapper or REST fallback.

Progress tracking is mandatory: immediately after start returns, visibly report the `run_id`, then relay every changed `status_report.current_stage`, `percent`, and `message` between bounded wait calls. MCP notifications, tool spinners, and hidden tool activity are not a substitute for a visible assistant update.
