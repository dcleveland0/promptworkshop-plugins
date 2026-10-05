# PromptWorkshop for ChatGPT and Codex

PromptWorkshop turns rough coding requests into scoped, testable instructions before implementation begins.

## Start here

1. Install and enable the PromptWorkshop plugin.
2. Authenticate when the MCP connection opens the PromptWorkshop sign-in flow.
3. Ask: "Harden this task before I start coding: <your task>."
4. Review the returned prompt before authorizing implementation.

The plugin connects to https://www.promptworkshop.io/api/mcp/v1. It does not bundle credentials or require a local PromptWorkshop server. Task text and any attachments or repository brief you choose to include are sent to PromptWorkshop for processing under its privacy policy.

For an explicitly invoked `/workshop-loop`, the host may execute and audit the exact downstream result for up to five rounds. A full cycle can incur charges for up to five optimize runs and five Result Audits, plus host execution. Result Audit evaluates caller-reported output; it does not execute code or independently verify tests. The separate `promptworkshop_loop_status` MCP tool reads durable Loop state when given a Loop ID; this host-managed workflow does not create that ID.

Manage suggestion levels — Off, Complex, Average, or Basic — in [PromptWorkshop External API settings](https://www.promptworkshop.io/settings/api). When a host activates the PromptWorkshop skill for an ordinary coding task, it can check this preference and ask before starting a run. Activation depends on the platform; no task text is sent before you say yes.

- Website: https://www.promptworkshop.io
- Privacy: https://www.promptworkshop.io/privacy
- Terms: https://www.promptworkshop.io/terms
- Support: info@promptworkshop.io
