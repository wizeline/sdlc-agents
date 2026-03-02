# Wize Skills

**Extend AI coding assistants with specialized expertise, procedural workflows, and task-specific resources.**

Wize Skills is a collection of agent skills built on the [Agent Skills](https://agentskills.io/home) open standard. It enhances AI coding assistants like Claude Code, Gemini, and Cursor by providing them with the context and tools they need for complex software engineering tasks.

---

## 🚀 Quick Start

> [!WARNING]
> **Already installed a previous version?** Delete your existing `skills/` and `agents/` directories before reinstalling. The installer does not merge or overwrite cleanly over existing files — leftover files from a previous version can cause skills to behave incorrectly or silently load stale instructions. See [Before you install: remove previous versions](#before-you-install-remove-previous-versions) below.

Install a skill for your preferred agent (Claude Code, Gemini CLI, etc.):

```bash
# General installation (interactive)
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering

# Target a specific agent
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code
```

> **Note**: For Gemini CLI users, you can also use `gemini skills install https://github.com/wizeline/wize-skills.git --path skills/docs-engineering`.

---

## 🗑️ Before you install: remove previous versions

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
