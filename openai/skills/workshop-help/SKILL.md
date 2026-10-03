---
name: workshop-help
description: Offline guide to writing effective prompts for PromptWorkshop and its agent integrations.
---

# Workshop Help

When invoked as `$workshop-help`, teach the user how to write a useful prompt. Arguments are optional topics for the explanation, never an optimization request. This help is self-contained and offline: do not load optimization instructions, call PromptWorkshop tools, inspect account state, or execute the described task.

Explain that a useful prompt names the outcome, relevant context, constraints, desired output, and observable proof of success. Audience and examples are optional; a rough prompt is enough to start. Keep it practical and concise.

**Before:** “Improve the onboarding.”

**After:** “For the mobile budgeting app, shorten first-time setup for people linking one bank account. Keep existing consent and security steps. Return the revised screen sequence and copy; success means a new user reaches the dashboard in under two minutes without skipping consent.”

**Template:**

```text
Help me [outcome] for [project or audience].
Context: [what matters].
Constraints: [what to keep, avoid, or ask first].
Return: [format or deliverable].
Done when: [observable success check].
```

Leave out irrelevant lines. `/workshop-status` handles connection diagnostics.
