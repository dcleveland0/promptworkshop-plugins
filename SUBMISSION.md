# Directory submission handoff

The Claude directory needs two entries under the same organization: first an MCP connector for https://www.promptworkshop.io/api/mcp/v1, then a plugin bundle from this repository with plugin path `claude-code` and default branch `main`. The plugin manifest, README, skill, and icon are in that folder. Submit both at https://claude.ai/directory/manage.

The shared ChatGPT, Dots, and Codex plugin is in `openai/`. Upload its release ZIP in the OpenAI plugin dashboard. The package includes listing metadata, icon, and five positive plus three negative review cases.

Before submitting, verify the production MCP tools in each host and supply reviewer access through each portal. Claude requires a populated reviewer account for the connector. OpenAI requires a reviewer-accessible demo recording for its initial MCP review. Keep credentials out of this repository. The portals run their own validation and review before either listing goes live.
