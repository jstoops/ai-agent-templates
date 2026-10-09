# Where to Discover MCP Servers

- [Official MCP Registry](https://registry.modelcontextprotocol.io/) - not the most active of places that you go to find MCP servers
- [MCP GitHub repo](https://github.com/modelcontextprotocol/servers) - repo put together by Anthropic with reference MCP servers, including the first ones they created plus third-party MCP servers that they have blessed. Good place to start by looking on this list and experimenting with some of these.
- [MCP.so](https://mcp.so/) - very active marketplace for MCP servers with search, keywords and tags make it easy to find what you're looking for and community comments and ratings, e.g. [context7](https://mcp.so/servers/context7-mcp) by upstash
- [Glama.ai MCP Directory](https://glama.ai/mcp) - another popular one with info like security, license and quality ratings, number of downloads, times favorited, e.g. [context7](https://glama.ai/mcp/servers/upstash/context7) by upstash

How do you tell if the MCP Server is legit? Go to the source, go to GitHub. See how many stars it has, is it being actively maintained, and review the README.md.

Useful MCP servers:
- [context7 by upstash](https://github.com/upstash/context7) - informs AI Coder about the latest information on APIs and packages
- [Atlassian MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/get-started-with-the-atlassian-remote-mcp-server/) - connect AI agents to Atlassian apps so you can work off tasks on your board, update the status, and more
- [GitHub's MCP Server](https://github.com/github/github-mcp-server) - create issues & lock down what actions the AI agents can perform on specific repos. Requires token: in GitHub.com select your Avatar->Settings->Developer settings->Personal access tokens->Fine-grained tokens then click Generate new token

# Where to Discover Skills

- [Skills GitHub repo](https://github.com/anthropics/skills) - Anthropic repo for skills that contains a template for creating your own skills and a folder of skills that they have written
- [Skills.sh by Vercel](https://www.skills.sh/) - made by Vercel, the people behind Next.js & Vercel deployment, including a skill to allow Claude Code to find other skills.
- [Skills by Matt Pocock](https://github.com/mattpocock/skills) - agent skills for real engineers

Useful skills:
- [agent-browser](https://www.skills.sh/vercel-labs/agent-browser/agent-browser) - browser automation
- [systematic-debugging](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging) - for finding and proving the root cause, which it calls the iron rule

# Where to Discover Plugins

- [Claude Code Plugins Directory GitHub repo](https://github.com/anthropics/claude-plugins-official)- see `plugins` for plugins written by Anthropic and `external_plugins` for plugins written by other people

Useful plugins:
- [ralph-loop](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-loop) - interactive self-referential AI loops for iterative development
- [frontend-design](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/frontend-design) - for creating production grade frontend interfaces
- [context7](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/context7) - informs AI Coder about the latest documentation on APIs and packages
- [code-review](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-review) - automated code review for pull requests
- [code-simplifier](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-simplifier) - agent that simplifies and refines code
- [feature-dev](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/feature-dev) - guide Claude Code through a disciplined 7 stage process to build a new feature for your product (love this!)