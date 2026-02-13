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
  - [Contributing](#contributing)
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
npx skills add https://github.com/wizeline/wize-skills --skill docs-engineering
```

Install all available skills:

```bash
npx skills add https://github.com/wizeline/wize-skills --skill '*'
```

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

| Skill Name          | Description                                                                 |
| ------------------- | --------------------------------------------------------------------------- |
| `docs-engineering`  | Documentation engineering workflows and quality assurance processes         |

> **Coming Soon**: More skills are in development. Check back regularly or watch this repository for updates.

---

## Usage Examples

### Interactive Installation

Install a skill with prompts to select your AI agent:

```bash
npx skills add https://github.com/wizeline/wize-skills --skill docs-engineering
```

### Non-Interactive Installation

Install for Claude Code without prompts:

```bash
npx skills add https://github.com/wizeline/wize-skills --skill docs-engineering -a claude-code -y
```

### Global Installation

Install globally for multiple agents:

```bash
npx skills add https://github.com/wizeline/wize-skills --skill docs-engineering -g -a claude-code -y
```

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

## Contributing

We welcome contributions! To add a new skill or improve existing ones:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/new-skill`)
3. Follow the [Agent Skills standard](https://agentskills.io/home) for skill structure
4. Submit a pull request

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
