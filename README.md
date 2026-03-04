# SDLC Agents

**Extend AI coding assistants with specialized expertise, procedural workflows, and task-specific resources.**

SDLC Agents is a collection of skills and subagents (referenced by folder as `agents/`) built on the [Agent Skills](https://agentskills.io/home) open standard. It enhances AI coding assistants like Claude Code, Gemini, and Cursor by providing them with the context and tools they need for complex software engineering tasks.

---

## 🚀 Quick Start

> [!WARNING]
> **Already installed a previous version?** Delete your existing `skills/` and `agents/` directories before reinstalling. The installer does not merge or overwrite cleanly over existing files — leftover files from a previous version can cause skills to behave incorrectly or silently load stale instructions. See [Before you install](#before-you-install) below.

Skills are installed via the **[`agents-skills`](https://www.npmjs.com/package/agents-skills)** CLI — the open agent skills & subagents ecosystem, compatible with Claude Code, Cursor, Gemini CLI, Codex, and 40+ other coding agents.

```bash
# Install (recommended)
npm i agents-skills

# Or install globally for repeated use
npm install -g agents-skills
```

Once available, add a skill or an agent for your preferred tool:

**Install a skill:**

```bash
# General installation (interactive) - select Skills (show all available in the folder)
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/

# Target a specific agent tool - specific skill (it also work for all skills, just delete the name of the specific skill)
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/authoring-technical-docs -a claude-code
```

**Install a subagent:**

```bash
# Install the doc-engineer subagent - all subagents
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents

# Target a specific agent tool - specific subagent (it also work for all subagents, just delete the name of the specific agent)
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents/doc-engineer -a claude-code

# Install an entire agent group at once
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents -a claude-code
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/security-agent/agents -a claude-code
```

> **IMPORTANT**: When installing, at the last step, select the option to install in the project NOT globally.

### ⚙️ Enabling Subagents

Some tools require additional configuration to use subagents:

#### Gemini CLI Setup

Subagents are currently experimental. To use custom subagents, you must enable them in your `settings.json` (located at `~/.gemini/settings.json` or project-root `.gemini/settings.json`):

```json
{
  "experimental": {
    "enableAgents": true
  }
}
```

#### 🔌 Configuring MCP Servers (Jira & Confluence)

To use agents like `atlassian-sourcer` that fetch data from external tools, you must configure the corresponding MCP servers in your `settings.json`. For Jira and Confluence, you can use the [Atlassian MCP server](https://mcpservers.org/servers/github-com-sooperset-mcp-atlassian):

```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://wizeline.atlassian.net",
        "JIRA_USERNAME": "your.email@wizeline.com",
        "JIRA_API_TOKEN": "your_api_token",
        "CONFLUENCE_URL": "https://wizeline.atlassian.net/wiki",
        "CONFLUENCE_USERNAME": "your.email@wizeline.com",
        "CONFLUENCE_API_TOKEN": "your_api_token"
      }
    }
  }
}
```

> **Note**: To get API tokens go to `https://id.atlassian.com/manage-profile/security/api-tokens`.
> **Note**: For Gemini CLI users, you can also use `gemini skills install https://github.com/wizeline/sdlc-agents.git --path aicores/documentation-writer-agent/skills/authoring-technical-docs`.

---

## Before you install

If you have previously installed any SDLC Agents, remove the existing `skills/` and `agents/` directories for your agent tool before running the installer again.

> **Back up first.** If you have made local customizations to any templates or reference files, copy them out before deleting. The reinstall overwrites the directory completely.

Skills and agents are stored in different locations depending on the tool and whether you installed at project scope (one project only) or user scope (available everywhere on your machine).

### Claude Code

```bash
# Project scope (inside your repo)
rm -rf .claude/skills/
rm -rf .claude/agents/

# User scope (global — affects all projects)
rm -rf ~/.claude/skills/
rm -rf ~/.claude/agents/
```

### Cursor

```bash
# Project scope
rm -rf .cursor/skills/
rm -rf .cursor/agents/

# User scope
rm -rf ~/.cursor/skills/
rm -rf ~/.cursor/agents/
```

> Cursor also reads `.agents/skills/` as a cross-tool alias. Remove it too if present:
>
> ```bash
> rm -rf .agents/skills/
> ```

### Gemini CLI

```bash
# Project scope
rm -rf .gemini/skills/
rm -rf .gemini/agents/
rm -rf .agents/skills/   # cross-tool alias

# User scope
rm -rf ~/.gemini/skills/
rm -rf ~/.gemini/agents/
rm -rf ~/.agents/skills/
```

### OpenAI Codex

```bash
# Project scope
rm -rf .codex/skills/

# User scope
rm -rf ~/.codex/skills/
```

### Not sure which scope you used?

Run the following to check all possible locations at once:

```bash
# Check project scope (run from your repo root)
ls -d .claude/skills/ .claude/agents/ \
       .cursor/skills/ .cursor/agents/ \
       .gemini/skills/ .gemini/agents/ \
       .agents/skills/ .codex/skills/ 2>/dev/null

# Check user scope
ls -d ~/.claude/skills/ ~/.claude/agents/ \
       ~/.cursor/skills/ ~/.cursor/agents/ \
       ~/.gemini/skills/ ~/.gemini/agents/ \
       ~/.agents/skills/ ~/.codex/skills/ 2>/dev/null
```

Any directory listed exists and should be removed before reinstalling.

---

## 📚 Learning & Examples

New to agent skills? Check out our comprehensive resources:

- **[Practical Tutorial](docs/tutorial-agent-skills-and-subagents.md)**: A deep dive into skills and subagents for Gemini, Claude, and Codex.
- **[Subagents FAQ](docs/FAQs.md#6-what-is-a-subagent)**: A short and concise explanation of what subagents are and how they work.
- **[Prompt Examples](docs/)**: A collection of curated prompts to see our skills in action:
  - [Documentation Engineering](docs/prompt-examples-docs-engineering.md)
  - [Developer Security Governance](docs/prompt-examples-devsec-governance.md)

---

## 🛠️ Available AI Cores

| Core Name | Description |
| :--- | :--- |
| [`documentation-writer-agent`](aicores/documentation-writer-agent) | Documentation engineering workflows and QA processes. |
| [`security-agent`](aicores/security-agent) | Developer security governance, OWASP, and compliance mapping. |
| [`unit-testing-agent`](aicores/unit-testing-agent) | Automated unit testing, coverage analysis, and test suite generation. |

> **Tip**: More AI cores are coming soon! Watch this repository for updates.

## 🤖 Featured Agents

| Agent Name | Core | Description |
| :--- | :--- | :--- |
| [`doc-engineer`](aicores/documentation-writer-agent/agents/doc-engineer.md) | `documentation-writer-agent` | Full documentation pipeline — research, draft, review, format, and export. |
| [`c4-architect`](aicores/documentation-writer-agent/agents/c4-architect.md) | `documentation-writer-agent` | Specialized C4 Model diagram generation. |
| [`atlassian-sourcer`](aicores/documentation-writer-agent/agents/atlassian-sourcer.md) | `documentation-writer-agent` | Fetches and structures content from Jira and Confluence via MCP. |
| [`devsec-code-review`](aicores/security-agent/agents/devsec-code-review.md) | `security-agent` | Security-focused code review against OWASP Top 10 and ASVS. |
| [`devsec-threat-modeling`](aicores/security-agent/agents/devsec-threat-modeling.md) | `security-agent` | STRIDE-based threat modeling for architecture designs. |
| [`devsec-architecture`](aicores/security-agent/agents/devsec-architecture.md) | `security-agent` | Security architecture for APIs, cloud-native, and AI/LLM systems. |
| [`devsec-ops-pipeline`](aicores/security-agent/agents/devsec-ops-pipeline.md) | `security-agent` | DevSecOps pipeline hardening and CI/CD security gates. |
| [`test-unit-gen-agent`](aicores/unit-testing-agent/agents/test-unit-gen-agent.md) | `unit-testing-agent` | Automated unit test generation and suite creation. |

---

## 🏗️ Repository Structure

- `aicores/`: Modularized AI cores, each containing its own `agents/` and `skills/`.
- `docs/`: Tutorials, prompt examples, and design documents.

---

## 🤝 Contributing & Versioning

We welcome contributions! Please follow the [Agent Skills standard](https://agentskills.io/home) when adding new skills.

- **Versioning**: Each AI Core is independently versioned using semantic versioning (`vMAJOR.MINOR.PATCH`).
- **Automation**: GitHub Actions automatically increment patch versions on every push to `main` for modified AI Cores.

To add a new AI Core, create a directory in the `aicores/` directory and seed it with an initial tag:

```bash
git tag "aicores/documentation-writer-agent/v1.0.0" && git push origin --tags
```

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

