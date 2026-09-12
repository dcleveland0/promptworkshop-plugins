---
name: workshop-auto
description: Harden and execute a task only when the user explicitly requests automatic execution.
---

# workshop-auto

Read ../promptworkshop/SKILL.md completely. Follow its shared authentication, privacy, transport, progress, failure, and output rules with these mode-specific requirements. Audit mode uses the audit lifecycle instead of optimize.

Treat explicit $workshop-auto invocation as pre-run authorization to skip the PromptWorkshop review stop. Parse optional --build <package_slug> before -- and preserve the task after -- exactly. Validate an explicit build with promptworkshop_list_packages before model spend.

Start once with handoff_mode: "auto_execute", workflow_tier: "full", format: "markdown_combined", include_harness: true, runRedTeam: true, and the optional validated package_slug. Override the parent's prompt-only format so the harness is returned with the prompt. Auto mode requires the full workflow; never infer depth from a slug. If repository context was requested, follow the parent's bounded attachment contract. Otherwise use the minimal request without repository content.

Verify successful provider-backed output and required phase completion; stop on failed, missing, or unverified output or source acceptance. Report "PromptWorkshop complete; downstream execution started" before implementing. Execute the hardened task and verify the resulting work. Auto mode does not waive host permissions, destructive-action approvals, credentials, billing, or workspace policy.
