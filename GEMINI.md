# Wize Skills Repository

This repository, `wize-skills`, serves as a centralized collection of AI Cores, each containing specialized agent skills and subagents designed to extend the capabilities of AI coding assistants like Gemini, Claude Code, Cursor, and others. These skills are built upon the [Agent Skills open standard](https://agentskills.io/home) and provide specialized expertise, procedural workflows, and task-specific resources to enhance various software engineering tasks.

## Purpose

The primary purpose of this repository is to:
- **Centralize AI Core Management**: Provide a single source for various AI Cores (Documentation, Security, Testing, etc.).
- **Enhance AI Assistant Capabilities**: Equip coding assistants with domain-specific knowledge, structured workflows, and relevant reference materials.
- **Promote Standardization**: Adhere to the Agent Skills open standard to ensure compatibility across different AI agents.
- **Automate Versioning**: Utilize GitHub Actions to automatically version and tag skills, ensuring discoverability and consistency.

## Repository Structure

The repository is organized by AI Core in the `aicores/` directory:

```
wize-skills/
├── .github/                      # GitHub Actions workflows (e.g., versioning)
├── aicores/                      # Modular AI Cores
│   ├── <core-name>/              # e.g., documentation-writer-agent, security-agent
│   │   ├── agents/               # Definition files for specific agents
│   │   └── skills/               # Directory containing all individual agent skills
│   │       ├── <skill-name>/     # Each subdirectory is a self-contained skill
│   │       │   ├── SKILL.md      # Main documentation for the skill
│   │       │   ├── assets/       # Supporting assets like templates
│   │       │   └── references/   # Detailed reference materials
├── docs/                         # General documentation and tutorials
├── .gitignore                    # Standard Git ignore file
├── LICENSE                       # Project license
├── README.md                     # High-level overview of the repository
└── sdlc-agents.code-workspace    # VSCode workspace configuration
```

## Available AI Cores

- **`documentation-writer-agent`**: Focuses on documentation engineering workflows and quality assurance processes.
- **`security-agent`**: A comprehensive core for developer security governance, covering OWASP Top 10, ASVS, and more.
- **`unit-testing-agent`**: Focuses on automated unit test generation and coverage analysis.

## Installing Skills

Skills are installed via the **[`agents-skills`](https://www.npmjs.com/package/agents-skills)** CLI — compatible with Gemini CLI, Claude Code, Cursor, Codex, and 40+ other coding agents.

```bash
# Run without installing (recommended)
npx agents-skills

# Or install globally for repeated use
npm install -g agents-skills
```

Add a skill to your project:

```bash
# General installation (interactive)
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/authoring-technical-docs

# Target Gemini specifically
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/authoring-technical-docs -a gemini

# Install globally (available across all projects)
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/authoring-technical-docs -g
```

Add a subagent to your project:

```bash
# Install a single subagent
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents/doc-engineer -a gemini

# Install an entire AI core agent group
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent -a gemini
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/security-agent -a gemini
```

## Contributing

Contributions are welcome. New skills can be added by creating a new directory under the appropriate AI Core's `skills/` directory with a `SKILL.md` file and any supporting resources. Refer to the `README.md` for detailed contribution guidelines and instructions on seeding initial version tags.

