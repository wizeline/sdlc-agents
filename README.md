# Wize Skills

**Extend AI coding assistants with specialized expertise, procedural workflows, and task-specific resources.**

Wize Skills is a collection of agent skills built on the [Agent Skills](https://agentskills.io/home) open standard. It enhances AI coding assistants like Claude Code, Gemini, and Cursor by providing them with the context and tools they need for complex software engineering tasks.

---

## 🚀 Quick Start

Install a skill for your preferred agent (Claude Code, Gemini CLI, etc.):

```bash
# General installation (interactive)
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering

# Target a specific agent
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code
```

> **Note**: For Gemini CLI users, you can also use `gemini skills install https://github.com/wizeline/wize-skills.git --path skills/docs-engineering`.

---

## 📚 Learning & Examples

New to agent skills? Check out our comprehensive resources:

- **[Practical Tutorial](docs/tutorial-agent-skills-and-subagents.md)**: A deep dive into skills and subagents for Gemini, Claude, and Codex.
- **[Prompt Examples](docs/)**: A collection of curated prompts to see our skills in action:
  - [Documentation Engineering](docs/prompt-examples-docs-engineering.md)
  - [Developer Security Governance](docs/prompt-examples-dev-security-governance.md)

---

## 🛠️ Available Skills

| Skill Name | Description |
| :--- | :--- |
| [`docs-engineering`](skills/docs-engineering) | Documentation engineering workflows and QA processes. |
| [`dev-security-governance`](skills/dev-security-governance) | Developer security governance, OWASP, and compliance mapping. |

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
