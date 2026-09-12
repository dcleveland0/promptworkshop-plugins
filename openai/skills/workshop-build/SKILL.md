---
name: workshop-build
description: Run a named PromptWorkshop saved build on a task. Use when the user invokes $workshop-build or explicitly selects a saved build slug.
---

# Workshop Build

Read `../promptworkshop/SKILL.md` completely, then follow it with these requirements:

1. Parse the first argument as `package_slug`; parse an optional `--level light|mid|full`; preserve the text after `--` exactly as the task. The level is a run-depth override, never a package slug.
2. If no slug is supplied, call `promptworkshop_list_packages`, show saved build slugs, and stop without optimizing.
3. Validate the slug before spending model tokens. If it is missing, show close available choices and stop.
4. Call native `promptworkshop_optimize_start` exactly once with `package_slug`, optional `workflow_tier`, `include_workflow: true`, the real project `cwd`, and `handoff_mode: "review_required"`; then use the same bounded `promptworkshop_optimize_wait` loop required by the parent skill.
5. Publish **Build prompt**, **Structure harness**, **Harden workflow**, and **Final QA**, name the selected build, show the hardened prompt, and wait for explicit approval.
6. `workflow_tier` is independent from `package_slug`: light = fast prompt-only/no red-team, mid = prompt+harness/no red-team, full = prompt+harness+red-team. Never infer a tier from a slug, because light/mid/full are legal custom slugs.
7. Never combine `package_slug` with `preset_slug`. Never use a wrapper or REST fallback.

Depth comes from the shared token grammar, never from the slug: with no digit pass `workflow_tier: "smart"`; `-1`, `-2`, `-3` pin `"light"`, `"mid"`, `"full"`. Never infer depth from the slug, because `light`, `mid`, and `full` are legal custom slugs. `--level light|mid|full` is still accepted as the older spelling of the same field.

Progress tracking is mandatory: immediately after start returns, visibly report the `run_id`, then relay every changed `status_report.current_stage`, `percent`, and `message` between bounded wait calls. MCP notifications, tool spinners, and hidden tool activity are not a substitute for a visible assistant update.
