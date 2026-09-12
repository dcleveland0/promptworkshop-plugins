---
name: workshop-build
description: Run a named PromptWorkshop saved build on a task.
---

# workshop-build

Read ../promptworkshop/SKILL.md completely. Follow its shared authentication, privacy, transport, progress, failure, and output rules with these mode-specific requirements. Audit mode uses the audit lifecycle instead of optimize.

Parse the first argument as package_slug, an optional --level light|mid|full (or -1/-2/-3) as workflow_tier, and preserve the task after -- exactly. With no depth override use "smart"; never infer depth from a slug. Light is prompt-only, mid adds a harness, and full adds red-team review.

Call promptworkshop_list_packages before starting. If no slug was supplied, show available slugs and stop. If the slug is invalid, show close available choices and stop before model spend. Start once with the validated package_slug, workflow_tier, include_workflow: true, and handoff_mode: "review_required". For mid/full override the parent's format with "markdown_combined" and include_harness: true; for light use "markdown_prompt_only" and include_harness: false. For smart, omit format and include_harness so the resolved tier controls both. Never combine package_slug with preset_slug. A saved build alone does not authorize reading the repository. If repository context was requested, use the parent's bounded attachment contract and a real repo.url only as optional matching metadata. Name the selected build with the result and stop for approval.
