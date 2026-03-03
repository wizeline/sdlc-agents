# Wize Skills Repository

This repository, `wize-skills`, serves as a centralized collection of agent skills designed to extend the capabilities of AI coding assistants like Gemini, Claude Code, Cursor, and others. These skills are built upon the [Agent Skills open standard](https://agentskills.io/home) and provide specialized expertise, procedural workflows, and task-specific resources to enhance various software engineering tasks.

## Purpose

The primary purpose of this repository is to:
- **Centralize Skill Management**: Provide a single source for various AI agent skills.
- **Enhance AI Assistant Capabilities**: Equip coding assistants with domain-specific knowledge, structured workflows, and relevant reference materials.
- **Promote Standardization**: Adhere to the Agent Skills open standard to ensure compatibility across different AI agents.
- **Automate Versioning**: Utilize GitHub Actions to automatically version and tag skills, ensuring discoverability and consistency.

## Repository Structure

The repository is organized to facilitate easy navigation and management of skills:

```
wize-skills/
├── .github/                      # GitHub Actions workflows (e.g., versioning)
├── agents/                       # Documentation or definitions for specific agents
├── docs/                         # General documentation about the project
├── skills/                       # Directory containing all individual agent skills
│   ├── <skill-name>/             # Each subdirectory is a self-contained skill
│   │   ├── SKILL.md              # Main documentation for the skill (YAML frontmatter + markdown)
│   │   ├── assets/               # Supporting assets like templates, images, etc.
│   │   └── references/           # Detailed reference materials for the skill
├── .gitignore                    # Standard Git ignore file
├── LICENSE                       # Project license
├── README.md                     # High-level overview of the repository
└── wize-skills.code-workspace    # VSCode workspace configuration
```

### Key Directories

- **`skills/`**: This is the core directory where all agent skills reside. Each sub-directory within `skills/` represents a distinct skill.
- **`<skill-name>/SKILL.md`**: Every skill must have a `SKILL.md` file at its root. This file contains YAML frontmatter defining the skill's `name` and `description`, followed by detailed markdown instructions on when and how the skill should be used, its workflow, deliverables, and key principles.
- **`<skill-name>/references/`**: This directory within each skill contains detailed reference materials (e.g., OWASP Top 10 documents, secure coding practices, compliance mappings) that the agent should consult for accurate and in-depth responses.

## Available Skills

Currently, the repository includes, but is not limited to, the following skills:

-   **`devsec-governance`**: A comprehensive skill for developer security governance, covering topics like OWASP Top 10, ASVS verification, NIST SSDF, CI/CD pipeline hardening, API/cloud-native security, and Security Champions programs. It provides guidance on secure development lifecycle, code reviews, threat modeling, and compliance.
-   **`docs-engineering`**: Focuses on documentation engineering workflows and quality assurance processes.

## Skill Versioning

Skills are independently versioned using semantic versioning (`vMAJOR.MINOR.PATCH`). A GitHub Actions workflow (`.github/workflows/version-skills.yml`) automatically increments the patch version of a skill whenever changes are pushed to `main` within its directory. Major and minor version bumps are performed manually via Git tags.

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
# Interactive installation
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering

# Target Gemini specifically
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a gemini

# Install globally (available across all projects)
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -g
```

For Gemini CLI's native installer:
```bash
gemini skills install https://github.com/wizeline/wize-skills.git --path skills/docs-engineering
```

Add a subagent to your project:

```bash
# Install a single subagent
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer -a gemini

# Install an entire agent group
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting -a gemini
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/dev-security -a gemini

# Install globally
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer -g
```

## Contributing

Contributions are welcome. New skills can be added by creating a new directory under `skills/` with a `SKILL.md` file and any supporting resources. Refer to the `README.md` for detailed contribution guidelines and instructions on seeding initial version tags.
