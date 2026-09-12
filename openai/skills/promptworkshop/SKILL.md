---
name: promptworkshop
description: Harden rough coding tasks with PromptWorkshop before implementation; use when the user asks for PromptWorkshop or wants a task made precise, testable, or agent-ready.
---

# PromptWorkshop for ChatGPT and Codex

Use the PromptWorkshop MCP tools to turn a rough coding request into a scoped, testable task. Authentication is handled by the host through the remote MCP connection. Never ask the user to paste credentials into chat.

## Workflow

1. Preserve the user's task text and intent.
2. For repo-aware work, inspect only the repository orientation files needed for the task, such as its agent rules, README, primary manifest, bounded file tree, and relevant module paths. Create a concise text brief named `repository-brief.md` and send it through `attachments`. Also pass a known git remote as `repo.url` for server-side saved-build matching. Never claim that a local directory path gave the remote service repository context.
3. Call `promptworkshop_optimize_start` exactly once with `handoff_mode: "review_required"`, then keep its `run_id`.
4. Poll only that run with `promptworkshop_optimize_wait` and `timeout_ms: 25000`. A timeout is resumable; it is not permission to create another run.
5. Show changed progress between waits using these phases: **Build prompt**, **Structure harness**, **Harden workflow**, and **Final QA**.
6. When the run succeeds, show only the hardened prompt under **Hardened prompt for review**, along with the run link and token usage when returned.
7. Stop for explicit approval before implementing. Skip this review stop only when the user requested automatic execution before the run; in that case use `handoff_mode: "auto_execute"`.

## Start call

```json
{
  "input": "<rough task>",
  "repo": { "url": "<git remote when known>" },
  "attachments": [
    { "name": "repository-brief.md", "kind": "text", "text": "<bounded repo brief when repo-aware work was requested>" }
  ],
  "expected_attachments": { "count": 1, "names": ["repository-brief.md"] },
  "prompt_starter": "goal",
  "target_model": "codex",
  "handoff_mode": "review_required",
  "format": "markdown_prompt_only",
  "include_harness": false,
  "runRedTeam": true
}
```

Use a fresh `idempotency_key` for a new user-intended run and reuse it only for a transport retry of that same start.

## Attachments

Remote MCP servers cannot read paths on the user's machine. Do not send `cwd` as a substitute for repository content. Extract visible files with the host's file or multimodal tools, then send text entries through `attachments`. Include `expected_attachments` with the exact count and sanitized names. If any source cannot be read completely, stop instead of sending a partial batch. Tell the user when repository text will be sent to PromptWorkshop.

## Failure handling

- If the durable start or wait tools are missing, report a plugin connection defect and stop. Do not replace the MCP call with a shell command or direct REST request.
- If a wait reports a stalled queue, keep the same `run_id`, stop polling, and report that the accepted job is not draining.
- If a result is marked as a no-cost test-agent response or reports zero provider tokens, do not present it as a real optimized prompt.
- Do not expose private provider routing fields, credentials, or raw tool envelopes.
