# Wize Skills

**Extend AI coding assistants with specialized expertise, procedural workflows, and task-specific resources.**

Wize Skills is a collection of agent skills built on the [Agent Skills](https://agentskills.io/home) open standard. Each skill is a self-contained directory that packages instructions, scripts, and assets into a discoverable capability that enhances AI coding assistants like Claude Code, Gemini, Cursor, and more.

---

## 📋 Table of Contents

- [Wize Skills](#wize-skills)
  - [📋 Table of Contents](#-table-of-contents)
  - [What Are Agent Skills?](#what-are-agent-skills)
  - [Installation](#installation)
    - [Quick Start](#quick-start)
    - [Installation Options](#installation-options)
    - [Alternative: Gemini CLI](#alternative-gemini-cli)
  - [Available Skills](#available-skills)
  - [Usage Examples](#usage-examples)
    - [Interactive Installation](#interactive-installation)
    - [Non-Interactive Installation](#non-interactive-installation)
    - [Global Installation](#global-installation)
    - [List Available Skills](#list-available-skills)
  - [Prerequisites](#prerequisites)
  - [Versioning](#versioning)
  - [Contributing](#contributing)
    - [Adding a New Skill](#adding-a-new-skill)
    - [Bumping Major or Minor Versions](#bumping-major-or-minor-versions)
  - [License](#license)

---

## What Are Agent Skills?

Agent Skills are modular capabilities that extend AI coding assistants with:

- **Specialized Expertise**: Domain-specific knowledge and best practices
- **Procedural Workflows**: Step-by-step processes for complex tasks
- **Task-Specific Resources**: Templates, scripts, and reference materials

Each skill follows the Agent Skills open standard, ensuring compatibility across 34+ AI coding agents including Claude Code, OpenCode, Codex, Cursor, and Antigravity.

---

## Installation

### Quick Start

> **💡 Tip**: For project-specific installations, navigate to your project directory in the terminal before running the installation commands.

Install a specific skill interactively:

```bash
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering
```

Install a specific skill for a specific agent:

```bash
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code
```

> **Note**: Point the URL directly to the skill's folder path within the repository (e.g., `.../tree/main/skills/docs-engineering`). For skills organized into subfolders (e.g., `docs-engineering/pdf`), each sub-skill must be installed individually using its full path (e.g., `.../tree/main/skills/docs-engineering/pdf`).

Install all available skills:

```bash
npx skills add https://github.com/wizeline/wize-skills --skill '*'
```

> **⚠️ Warning**: Omitting the `-a` / `--agent` flag when using `--all` or `--skill '*'` will install skills for **every supported agent** on your machine. To limit installation to a specific agent, always pass the `-a` flag (e.g., `-a claude-code`).

> **Note**: No installation of `npx skills` is required—it's a zero-install tool that runs directly via Node.js (v18+). Learn more about [npx skills](https://github.com/vercel-labs/skills/tree/main?tab=readme-ov-file#other-commands).

### Installation Options

| Option                    | Description                                                                                          |
| ------------------------- | ---------------------------------------------------------------------------------------------------- |
| `-g, --global`            | Install to user directory (`~/.skills`) instead of project directory                                |
| `-a, --agent <agents...>` | Target specific agents (e.g., `claude-code`, `codex`, `cursor`)                                      |
| `-s, --skill <skills...>` | Install specific skills by name (use `'*'` for all skills)                                          |
| `-l, --list`              | List available skills without installing                                                             |
| `-y, --yes`               | Skip all confirmation prompts                                                                        |
| `--all`                   | Install all skills to all agents without prompts                                                     |

### Alternative: Gemini CLI

If you're using Google's Gemini agent, you can install skills using the Gemini CLI:

```bash
# Install to user scope (default: ~/.gemini/skills)
gemini skills install https://github.com/wizeline/wize-skills.git --path skills/docs-engineering

# Install to workspace scope (.gemini/skills)
gemini skills install https://github.com/wizeline/wize-skills.git --path skills/docs-engineering --scope workspace
```

---

## Available Skills

| Skill Name                 | Description                                                                 |
| -------------------------- | --------------------------------------------------------------------------- |
| `docs-engineering`         | Documentation engineering workflows and quality assurance processes         |
| `dev-security-governance`  | Development security governance checks and best practices                   |

> **Coming Soon**: More skills are in development. Check back regularly or watch this repository for updates.

---

## Usage Examples

### Interactive Installation

Install a skill with prompts to select your AI agent:

```bash
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering
```

### Non-Interactive Installation

Install for Claude Code without prompts:

```bash
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code
```

### Global Installation

Install globally for a specific agent:

```bash
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code -g
```

### Install All Skills for a Specific Agent

Install every available skill for a single agent:

```bash
npx skills add https://github.com/wizeline/wize-skills --skill '*' -a claude-code -y
```

> **⚠️ Warning**: Dropping the `-a` flag installs all skills for **all supported agents**. Always specify `-a <agent>` unless you intentionally want a system-wide installation.

### List Available Skills

Preview all available skills before installing:

```bash
npx skills add https://github.com/wizeline/wize-skills --list
```

---

## Prerequisites

- **Node.js v18+**: Required for `npx` (bundled with Node.js)
  - Check your version: `node --version`
  - Download from [nodejs.org](https://nodejs.org) if needed
- **AI Coding Agent**: One of the 34+ supported agents (Claude Code, Cursor, etc.)

---

## Versioning

Every skill is independently versioned using **semantic versioning** (`v1.x.x`). Tags follow the pattern:

```
skills/<skill-name>/v<MAJOR>.<MINOR>.<PATCH>
```

For example: `skills/docs-engineering/v1.0.0`, `skills/dev-security-governance/v1.2.3`.

### How It Works

- A **GitHub Actions workflow** (`.github/workflows/version-skills.yml`) runs on every push to `main` that modifies files under `skills/`.
- It detects which skill changed, looks up the **latest existing tag** for that skill, and creates a new tag with the **patch version incremented** (`v1.0.0` → `v1.0.1` → `v1.0.2` …).
- If no tag exists yet for a skill, it starts at `v1.0.0`.

> **Note**: Hidden files like `.DS_Store` are automatically filtered out and will never produce tags.

---

## Contributing

We welcome contributions! To add a new skill or improve existing ones:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/new-skill`)
3. Follow the [Agent Skills standard](https://agentskills.io/home) for skill structure
4. Submit a pull request

### Adding a New Skill

1. Create a new directory under `skills/` with your skill name (e.g., `skills/my-new-skill/`).
2. Add a `SKILL.md` file with the required YAML frontmatter (`name`, `description`) and detailed instructions.
3. Include any supporting directories (`scripts/`, `examples/`, `resources/`) as needed.
4. **Seed the initial version tag** before your first merge to `main`:

   ```bash
   git tag "skills/my-new-skill/v1.0.0"
   git push origin "skills/my-new-skill/v1.0.0"
   ```

5. After seeding, all subsequent pushes to `main` that modify your skill will **auto-increment** the patch version.

### Bumping Major or Minor Versions

The CI workflow only auto-increments the **patch** version. To bump the **minor** or **major** version, create the tag manually:

```bash
# Bump minor version (e.g., v1.0.5 → v1.1.0)
git tag "skills/my-skill/v1.1.0"
git push origin "skills/my-skill/v1.1.0"

# Bump major version (e.g., v1.3.2 → v2.0.0)
git tag "skills/my-skill/v2.0.0"
git push origin "skills/my-skill/v2.0.0"
```

The next automated push will continue incrementing from your new tag (e.g., `v2.0.1`).

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
 