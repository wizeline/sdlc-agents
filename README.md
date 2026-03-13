# SDLC Agents

**Extend AI coding assistants with specialized expertise, procedural workflows, and task-specific resources.**

SDLC Agents is a collection of reusable skills and [subagents](docs/FAQs.md#6-what-is-a-subagent) built on the [Agent Skills](https://agentskills.io/home) open standard. It enhances AI coding assistants like Claude Code, Gemini, and Cursor by equipping them with the context and tools needed for complex software engineering tasks.

Skills and agents are organized into two structures:

- **AI Cores** (`aicores/` folder): Groups of related skills and agents that work together toward a shared goal
- **Standalone components** (`skills/` and `agents/` folders): Individual skills or agents that operate independently

Components can belong to an AI Core or exist as standalone modules—they don't need to be co-located.

---

## 📖 Tutorial: How to Configure and Use SDLC AI Cores & Skills

**AI Cores** are accelerators designed to help you implement AI into your software development lifecycle. They group together **Agents** (like Gemini, Cloud Code, Cursor, or Windsurf) and **Skills** (specific tasks formatted as Markdown files) to automate common workflows like Documentation, Security, Unit Testing, and Incident Management.

Here is how to set them up and use them in your daily workflow.

### 📋 Prerequisites

Before you begin, ensure you have access to the repository:

1. Link your GitHub account with your Wizeline email.
2. Create a ticket to link your account to the Wizeline GitHub organization (`wizeline.org`).
3. Have an AI Assistant installed in your IDE (e.g., Gemini, Cloud Code, Cursor, or Windsurf).

---

### ⚙️ Step 1: Install the AI Cores

Instead of manually copying and pasting files, you can use the provided TypeScript CLI tool to install specific AI Cores directly into your project.

1. Open your terminal in the root directory of your project.
2. Run the following command, replacing the URL with the specific AI Core you want to install (e.g., the Documentation core):

   ```bash
   npx ai-cores add https://github.com/wizeline/sdlc-agents/tree/main/cores/documentation
   ```

3. **Follow the CLI prompts:** The tool will ask you which specific agents/skills you want to use. You can select all of them.
4. **Select your AI Assistant:** Choose the assistants you are currently using (e.g., `Gemini`, `Cloud Code`, `Cursor`, or `Windsurf`).
5. The tool will automatically create a `.agents` (or `.cursor/agents`) folder in your project root containing all the necessary Markdown files.

---

### 🔒 Step 2: Update your `.gitignore` (Crucial!)

These AI Cores are internal Wizeline accelerators to help *you* work faster. **They should not be pushed to the client's repository.**

Immediately open your project's `.gitignore` file and add the following lines to prevent committing the agent files:

```text
# AI Agents & Skills
.agents/
.cursor/agents/
```

---

### 🛠️ Step 3: Using the Skills in your IDE

Once installed, your AI Assistant will automatically detect the skills.

1. **List Available Skills:** Open your AI Assistant's chat window (Gemini, Cloud Code, Cursor, etc.) and type the command `/skills` (or `/agents` in Cloud Code). It will list all the skills you just installed.
2. **Trigger a Skill:** You don't always need to call a skill by its exact name. You can simply write a prompt with a clear intent, and the AI will trigger the appropriate Markdown skill file.
   - *Example:* `"Document the architecture of this project using the C4 model."`
3. **Automated Workflows:** Some skills can be triggered by actions. For example, if you have the documentation core installed, you can prompt: *"git commit changes and push them,"* and the AI can automatically trigger the skill to update the documentation alongside your commit.

---

### 💡 Pro-Tips & Best Practices

- **Save Tokens by Being Specific:** While you *can* use ambiguous prompts (e.g., "Document the project"), this will consume a massive amount of tokens as the AI scans the entire codebase. Instead, use specific prompts (e.g., "Generate unit tests for `auth_service.java`") to save tokens and get faster results.
- **Set Global Rules with Context Files:** If you want to set global rules (like token limits, tone of voice, or project context), you don't need to create a new skill. Instead, create a context file in your root folder named after your assistant (e.g., `gemini.md`, `cloud.md`, or `cursor.md`). The AI will read this file automatically before executing tasks.
- **Clean Updates:** If a new version of an AI Core is released, the safest way to update is to manually delete the `.agents` folder and re-run the `npx ai-cores add` command to ensure a clean installation.
- **Connect to Jira/Confluence:** If you need your agents to read tickets or wikis, check the [MCP Servers configuration](#-configuring-mcp-servers-jira-confluence--github) section below on how to configure **MCP (Model Context Protocol)** locally to connect your agents to Jira, Confluence, or GitHub.

---

## 🛠️ Available AI Cores

| Core Name                                                           | Description                                                                      |
| :------------------------------------------------------------------ | :------------------------------------------------------------------------------- |
| [`documentation-writer-agent`](aicores/documentation-writer-agent)    | Documentation engineering workflows and QA processes.                            |
| [`security-agent`](aicores/security-agent)                          | Developer security governance, OWASP, and compliance mapping.                    |
| [`code-review-agent`](aicores/code-review-agent)                    | Autonomous code reviews across security, performance, and maintainability.       |
| [`unit-testing-agent`](aicores/unit-testing-agent)                  | Automated unit testing, coverage analysis, and test suite generation.            |
| [`incident-resolution-agent`](aicores/incident-resolution-agent)    | Incident triage, root cause analysis, remediation, and postmortem documentation. |

> **Tip**: More AI cores like **QA Agent** are coming soon! Watch this repository for updates.

## 🤖 Featured Agents

| Agent Name                                                                                    | Core                         | Description                                                                        |
| :-------------------------------------------------------------------------------------------- | :--------------------------- | :--------------------------------------------------------------------------------- |
| [`doc-engineer`](aicores/documentation-writer-agent/agents/doc-engineer.md)                   | `documentation-writer-agent` | Full documentation pipeline — research, draft, review, format, and export.         |
| [`c4-architect`](aicores/documentation-writer-agent/agents/c4-architect.md)                   | `documentation-writer-agent` | Specialized C4 Model diagram generation.                                           |
| [`atlassian-sourcer`](aicores/documentation-writer-agent/agents/atlassian-sourcer.md)         | `documentation-writer-agent` | Fetches and structures content from Jira and Confluence via MCP.                   |
| [`code-reviewer`](aicores/code-review-agent/agents/code-reviewer.md)                          | `code-review-agent`          | Autonomous pipeline for security, performance, and maintenance reviews.            |
| [`devsec-code-review`](aicores/security-agent/agents/devsec-code-review.md)                   | `security-agent`             | Security-focused code review against OWASP Top 10 and ASVS.                        |
| [`devsec-threat-modeling`](aicores/security-agent/agents/devsec-threat-modeling.md)           | `security-agent`             | STRIDE-based threat modeling for architecture designs.                             |
| [`devsec-architecture`](aicores/security-agent/agents/devsec-architecture.md)                 | `security-agent`             | Security architecture for APIs, cloud-native, and AI/LLM systems.                  |
| [`devsec-ops-pipeline`](aicores/security-agent/agents/devsec-ops-pipeline.md)                 | `security-agent`             | DevSecOps pipeline hardening and CI/CD security gates.                             |
| [`devsec-compliance-framework`](aicores/security-agent/agents/devsec-compliance-framework.md) | `security-agent`             | Compliance mapping and gap analysis for ISO 27001, SOC 2, PCI-DSS, HIPAA, and more. |
| [`devsec-program`](aicores/security-agent/agents/devsec-program.md)                           | `security-agent`             | Security program maturity assessment, OWASP SAMM, and Security Champions planning.  |
| [`test-unit-gen-agent`](aicores/unit-testing-agent/agents/test-unit-gen-agent.md)             | `unit-testing-agent`         | Automated unit test generation and suite creation.                                 |
| [`test-unit-review-agent`](aicores/unit-testing-agent/agents/test-unit-review-agent.md)       | `unit-testing-agent`         | Quality gate for generated test suites — reviews correctness, coverage, and style.  |
| [`incident-commander`](aicores/incident-resolution-agent/agents/incident-commander-agent.md)  | `incident-resolution-agent` | Orchestrates triage, RCA, remediation, and postmortem for any SDLC incident.       |

---

## 🚀 Quick Start

> [!WARNING]
> **Already installed a previous version?** Delete your existing `skills/` and `agents/` directories before reinstalling. The installer does not merge or overwrite cleanly over existing files — leftover files from a previous version can cause skills to behave incorrectly or silently load stale instructions. See [Before you install](#before-you-install) below.

Skills and agents are installed via the **[`aicore-cli`](https://github.com/wizeline/aicore-cli)** — an open agent ecosystem CLI for installing agents and skills across Claude Code, Cursor, Gemini CLI, Codex, and 40+ other coding agents.

### Install Agents & Skills Together

[optional] First, install the `aicore-cli`:

```bash
npm install -g aicores
```

Then use `npx aicore` to install both agents and skills from an aicore package in one command:

```bash
# Install all agents and skills from SDLC Agents interactively
npx aicore add wizeline/sdlc-agents/aicore-name

# Or with the GitHub URL
npx aicore add https://github.com/wizeline/sdlc-agents/aicore-name

# Install to a specific AI assistant
npx aicore add wizeline/sdlc-agents/aicore-name -a claude-code

# Install to multiple assistants
npx aicore add wizeline/sdlc-agents/aicore-name -a claude-code -a cursor

# List available items before installing
npx aicore wizeline/sdlc-agents --list
```

### Install Agents or Skills Separately

Use `npx subagents` or `npx skills` to install items independently:

```bash
# Install all agents from a specific core
npx subagents add wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents

# Install a specific agent
npx subagents add wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents/doc-engineer -a claude-code

# Install all skills from a specific core
npx skills add wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/

# Install a specific skill
npx skills add wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/authoring-technical-docs -a claude-code
```

> **IMPORTANT**: When installing, select the option to install in the project (not globally) unless you want the agents/skills available across all projects.
>
> **⚠️ WARNING - Wizeline Usage Only**: Please update the `.gitignore` of your client repo in which these skills and agents will be installed so they won't be part of the source code. This is Wizeline usage only.

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

#### 🔌 Configuring MCP Servers (Jira, Confluence & GitHub)

Some agents fetch data from external tools and require MCP servers configured in your `settings.json`.

**Atlassian (Jira & Confluence)** — used by `atlassian-sourcer`, `incident-commander`, and others. Use the [Atlassian MCP server](https://mcpservers.org/servers/github-com-sooperset-mcp-atlassian):

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

> **Note**: To get Atlassian API tokens go to `https://id.atlassian.com/manage-profile/security/api-tokens`.

**GitHub** — used by `incident-commander` to inspect repos, PRs, and CI runs during incident response. Use the [GitHub MCP server](https://github.com/github/github-mcp-server):

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "ghcr.io/github/github-mcp-server"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_github_pat"
      }
    }
  }
}
```

> **Note**: Create a GitHub Personal Access Token at `https://github.com/settings/tokens`. The token needs `repo` and `read:org` scopes for most incident response operations.

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

- **[Practical Tutorial](docs/tutorial-aicores-agents-skills.md)**: A deep dive into skills and subagents for Gemini, Claude, and Codex.
- **[Subagents FAQ](docs/FAQs.md#6-what-is-a-subagent)**: A short and concise explanation of what subagents are and how they work.
- **[Prompt Examples](docs/)**: A collection of curated prompts to see our skills in action:
  - [Documentation Writer](docs/prompt-examples-doc-writer-agent.md)
  - [Code Review](docs/prompt-examples-code-review-agent.md)
  - [Security](docs/prompt-examples-security-agent.md)
  - [Unit Testing](docs/prompt-examples-unit-testing-agent.md)
  - [Incident Support](docs/prompt-examples-incident-support-agent.md)

---

## 🏗️ Repository Structure

- `aicores/`: Modularized AI cores, each containing its own `agents/` and `skills/`.
- `docs/`: Tutorials, prompt examples, and design documents.

---

## 🤝 Contributing & Versioning

We welcome contributions! Please adhere to the [Agent Skills standard](https://agentskills.io/home) when adding new skills, and follow Anthropic's [subagent format](https://code.claude.com/docs/en/sub-agents#write-subagent-files) when adding agents.

## Contribution Guidelines

### 1. Branch Naming & Organization

**For general-purpose skills/agents** (available across Wizeline projects):

- Create a branch from `main` using the appropriate pattern:

  - `aicore/aicore_name` — for both skills and agents with the same purpose
  - `skills/skill_name` — for standalone skills only
  - `agents/agent_name` — for standalone agents only
- If creating both skills and agents together, group them in a single folder within `aicores/` so they can be installed via **[`aicore-cli`](https://github.com/wizeline/aicore-cli)**
- Standalone skills belong in the `skills/` folder; standalone agents in the `agents/` folder

**For account-specific skills/agents** (e.g., UTA, KOF):

- Create a branch using the pattern: `account_name/aicore/skill/agent_name`

### 2. AI Core Structure

An AI Core follows this directory structure:

```text
my-aicore/
├── agents/
│   └── agent-name.md        ← Required: YAML frontmatter + instructions
└── skills/
    └── skill-name/
        ├── SKILL.md          ← Required: YAML frontmatter + instructions
        ├── references/
        │   └── reference.md  ← Optional: reference documents
        ├── assets/
        │   └── template.md   ← Optional: template files
        └── scripts/
            └── helper.py     ← Optional: helper scripts
```

**Required files:**

- `agents/*.md` — Agent definitions with YAML frontmatter and instructions
- `skills/*/SKILL.md` — Skill definitions with YAML frontmatter and instructions

**Optional directories:**

- `references/` — Supporting documentation and reference materials
- `assets/` — Template files and reusable assets
- `scripts/` — Helper scripts and utilities

### 3. Versioning & Automation

- Each AI Core uses independent semantic versioning (`vMAJOR.MINOR.PATCH`)
- GitHub Actions automatically increments patch versions on every push to `main` for modified AI Cores

### 4. Creating a New AI Core

Initialize a new AI Core directory with an initial version tag:

```bash
git tag "aicores/aicore_name/v1.0.0" && git push origin --tags
```

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
