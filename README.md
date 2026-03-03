# Wize Skills

**Extend AI coding assistants with specialized expertise, procedural workflows, and task-specific resources.**

Wize Skills is a collection of agent skills built on the [Agent Skills](https://agentskills.io/home) open standard. It enhances AI coding assistants like Claude Code, Gemini, and Cursor by providing them with the context and tools they need for complex software engineering tasks.

---

## 🚀 Quick Start

> [!WARNING]
> **Already installed a previous version?** Delete your existing `skills/` and `agents/` directories before reinstalling. The installer does not merge or overwrite cleanly over existing files — leftover files from a previous version can cause skills to behave incorrectly or silently load stale instructions. See [Before you install](#before-you-install) below.

Skills are installed via the **[`agents-skills`](https://www.npmjs.com/package/agents-skills)** CLI — the open agent skills & subagents ecosystem, compatible with Claude Code, Cursor, Gemini CLI, Codex, and 40+ other coding agents.

```bash
# Run without installing (recommended)
npx agents-skills

# Or install globally for repeated use
npm install -g agents-skills
```

Once available, add a skill or an agent for your preferred tool:

**Install a skill:**

```bash
# General installation (interactive)
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering

# Target a specific agent tool
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code

# Install globally (available across all your projects)
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -g
```

**Install a subagent:**

```bash
# Install the doc-engineer subagent
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer

# Target a specific agent tool
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer -a claude-code

# Install globally
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer -g

# Install an entire agent group at once
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting -a claude-code
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/dev-security -a claude-code
```

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
> **Note**: For Gemini CLI users, you can also use `gemini skills install https://github.com/wizeline/wize-skills.git --path skills/docs-engineering`.

---

## Before you install

If you have previously installed any Wize Skills, remove the existing `skills/` and `agents/` directories for your agent tool before running the installer again.

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
- **[Prompt Examples](docs/)**: A collection of curated prompts to see our skills in action:
  - [Documentation Engineering](docs/prompt-examples-docs-engineering.md)
  - [Developer Security Governance](docs/prompt-examples-devsec-governance.md)

---

## 🛠️ Available Skills

| Skill Name | Description |
| :--- | :--- |
| [`docs-engineering`](skills/docs-engineering) | Documentation engineering workflows and QA processes. Includes multi-agent support (`doc-engineer`, `c4-architect`, `atlassian-sourcer`) and comprehensive skills like `authoring-api-docs`. |
| [`devsec-governance`](skills/devsec-governance) | Developer security governance, OWASP, and compliance mapping. |

> **Tip**: More skills are coming soon! Watch this repository for updates.

## 🤖 Available Agents

| Agent Name | Group | Description |
| :--- | :--- | :--- |
| [`doc-engineer`](agents/documenting/doc-engineer.md) | `documenting` | Full documentation pipeline — research, draft, review, format, and export. |
| [`c4-architect`](agents/documenting/c4-architect.md) | `documenting` | Specialized C4 Model diagram generation. |
| [`atlassian-sourcer`](agents/documenting/atlassian-sourcer.md) | `documenting` | Fetches and structures content from Jira and Confluence via MCP. |
| [`devsec-code-review`](agents/dev-security/devsec-code-review.md) | `dev-security` | Security-focused code review against OWASP Top 10 and ASVS. |
| [`devsec-threat-modeling`](agents/dev-security/devsec-threat-modeling.md) | `dev-security` | STRIDE-based threat modeling for architecture designs. |
| [`devsec-architecture`](agents/dev-security/devsec-architecture.md) | `dev-security` | Security architecture for APIs, cloud-native, and AI/LLM systems. |
| [`devsec-ops-pipeline`](agents/dev-security/devsec-ops-pipeline.md) | `dev-security` | DevSecOps pipeline hardening and CI/CD security gates. |
| [`devsec-compliance-framework`](agents/dev-security/devsec-compliance-framework.md) | `dev-security` | Compliance mapping across ISO 27001, SOC 2, PCI-DSS, and more. |
| [`devsec-program`](agents/dev-security/devsec-program.md) | `dev-security` | AppSec program building and OWASP SAMM maturity assessments. |

---

## 🏗️ Repository Structure

- `skills/`: Self-contained agent skills (instructions, scripts, and assets).
- `docs/`: Tutorials, prompt examples, and design documents.
- `agents/`: Definitions for specific agent configurations.

---

## 🤝 Contributing & Versioning

We welcome contributions! Please follow the [Agent Skills standard](https://agentskills.io/home) when adding new skills.

- **Versioning**: Each skill is independently versioned using semantic versioning (`vMAJOR.MINOR.PATCH`).
- **Automation**: GitHub Actions automatically increment patch versions on every push to `main` for modified skills.

To add a new skill, create a directory in `skills/` with a `SKILL.md` file and seed it with a tag:

```bash
git tag "skills/your-skill/v1.0.0" && git push origin --tags
```

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
