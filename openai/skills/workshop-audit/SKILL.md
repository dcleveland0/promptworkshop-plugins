---
name: workshop-audit
description: Audit an agent result against a prior PromptWorkshop run.
---

# workshop-audit

Read ../promptworkshop/SKILL.md completely. Follow its shared authentication, privacy, transport, progress, failure, and output rules with these mode-specific requirements. Audit mode uses the audit lifecycle instead of optimize.

Parse the first argument as run_id and an optional approved, rejected, or unsure token as human_verdict. Use the remaining text or the clearly applicable preceding agent response as downstream_response; ask only if neither exists.

Check the audit start and wait tools and authentication before starting. Call promptworkshop_result_audit_start once, including downstream client/model when known. Do not pass target_model_id. Keep audit_id and poll promptworkshop_result_audit_wait with that same run_id and audit_id and timeout_ms: 25000. A timeout resumes the same audit; never start a duplicate. Report changed progress between waits, then verdict, scores, failure modes, revised-prompt recommendation, and evolution focus. Do not claim this audit executed tests or independently verified the implementation.

Create a child run only after explicit approval to adopt the revision, by calling promptworkshop_result_audit_adopt with the same IDs. Return open_in_promptworkshop. A deliberate re-audit of byte-identical output uses a fresh idempotency_key; ordinary transport retries retain the same key or omit it to replay safely. Missing tools, denied permissions, typed preflight failures, and stalled queues stop the workflow. Never use a wrapper, REST fallback, or blocking compatibility tool.
