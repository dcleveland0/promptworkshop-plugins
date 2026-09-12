---
name: workshop-audit
description: Audit an agent result against a prior PromptWorkshop run. Use when the user invokes $workshop-audit or supplies a run id and downstream response for Result Audit.
---

# Workshop Audit

1. Parse the first argument as `run_id`.
2. Parse an optional next token of `approved`, `rejected`, or `unsure` as `human_verdict`.
3. Use the remaining text, or the immediately preceding agent response when clearly applicable, as `downstream_response`. Ask for it only when neither exists.
4. Call native `promptworkshop_result_audit_start` exactly once; include downstream client/model when known. Do not pass `target_model_id`: durable audit uses the supported background judge.
5. Preserve the returned `audit_id`. Call `promptworkshop_result_audit_wait` with the same `run_id` and `audit_id`, using `timeout_ms: 25000`. After `status: "timeout"`, repeat bounded waits with those same ids; never start a duplicate.
6. Return verdict, scores, failure modes, revised-prompt recommendation, and evolution focus.
7. Do not create a child run unless the user explicitly asks to adopt the revision. When they do, call `promptworkshop_result_audit_adopt` with the same `run_id` and `audit_id`, then return `open_in_promptworkshop`.
8. If an intentional re-audit of byte-identical output is requested, pass a fresh `idempotency_key` to start. Omit it for ordinary retries so they replay safely.
9. If any lifecycle MCP tool is unavailable, report the registration defect and stop. Never use a wrapper, REST fallback, or the blocking compatibility tool.
