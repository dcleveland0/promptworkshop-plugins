---
name: workshop-repo
description: Harden a task with a bounded repository brief when explicitly requested.
---

# workshop-repo

Read ../promptworkshop/SKILL.md completely. Follow its shared authentication, privacy, transport, progress, failure, and output rules with these mode-specific requirements. Audit mode uses the audit lifecycle instead of optimize.

Preserve the task and resolve a credential-free git remote when available. Follow the parent's bounded repository attachment contract: repo-context-notes.md, kind spec, expected_attachments, and include_repo_context: false. Never request a remote scan of a local directory.

Call promptworkshop_list_repo_packages when available. Match saved builds in order, stopping at the first tier with exactly one candidate: normalized exact repo URL (normalize SSH/HTTPS spelling, trailing .git, and case); the user's explicit search hint against slug, name, and bound URL; the same org/repo on another host; the same repo name under another owner. Label path or name matches as guesses and name the matching tier. For multiple candidates, show the shortlist and stop. If no build matches, report Build: account default and show the detected URL and available bindings. Pass a matched package_slug and a real repo.url only as matching metadata. Do not infer depth from a slug or set runRedTeam independently.

With no depth digit use workflow_tier: "smart"; -1, -2, and -3 select "light", "mid", and "full". For mid/full override the parent's format with "markdown_combined" and include_harness: true; for light use "markdown_prompt_only" and include_harness: false. For smart, omit format and include_harness so the resolved tier controls both. Report accepted sources with the result, then stop for approval.
