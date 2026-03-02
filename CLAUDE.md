# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Wize Skills is a monorepo of modular Agent Skills built on the [Agent Skills open standard](https://agentskills.io/home). Skills are self-contained packages of instructions, references, and templates that extend AI coding assistants (Claude Code, Cursor, Gemini, etc.) with specialized expertise. Skills are **documentation-based** — there is no application code, build step, or test suite.

## Repository Structure

```
skills/
  {skill-name}/
    SKILL.md              # Skill definition: triggers, workflow, methodology (YAML frontmatter + markdown)
    references/           # Domain-specific knowledge, templates, best practices
    agents/               # Subagent instructions (for multi-agent workflows)
    assets/               # Reusable output templates
agents/                   # Top-level Claude Code agent orchestrators
docs/                     # Design documents for skills
```

### Current Skills

- **docs-engineering** — Multi-agent documentation pipeline (Researcher → Writer → Reviewer → Formatter) using the Diátaxis framework and first-party consumption testing
- **devsec-governance** — SDLC security guidance powered by OWASP Top 10 (2025), NIST SSDF, ASVS 5.0, and 12 reference frameworks

## Key Architecture Concepts

**SKILL.md** is the entry point for each skill. It contains YAML frontmatter (`name`, `description`, `context`, `agent`) and markdown sections defining when the skill triggers, its core workflow, framework selection tables, and output format guidelines.

**Multi-agent orchestration** (docs-engineering): Specialized subagents in `agents/` each handle one phase. They communicate via intermediate markdown artifacts. The reviewer subagent validates docs by attempting to complete a task using *only* the document (first-party consumption testing), with up to 2 revision cycles.

**Reference files** are static knowledge bases that skills route to via decision tables in SKILL.md. The devsec-governance skill routes questions like "What are top risks?" → `owasp-top10-2025.md`, "How mature is our program?" → `samm-maturity.md`, etc.

## Versioning & CI

Each skill is independently versioned via git tags: `skills/{skill-name}/v{MAJOR}.{MINOR}.{PATCH}`

The GitHub Actions workflow (`.github/workflows/version-skills.yml`) auto-increments the **patch** version on every push to `main` that modifies files under `skills/`. If no tag exists for a skill, it starts at `v1.0.0`. Hidden files (e.g., `.DS_Store`) are filtered out.

For major/minor bumps, create tags manually:
```bash
git tag "skills/my-skill/v2.0.0"
git push origin "skills/my-skill/v2.0.0"
```

PR builds produce tags with a suffix: `v1.0.1-pr.42.a3f8b2c`

## Adding a New Skill

1. Create `skills/my-skill/` with at minimum a `SKILL.md` containing YAML frontmatter (`name`, `description`) and sections for triggers, workflow, and methodology
2. Add `references/` and optionally `agents/` and `assets/` directories
3. Seed the initial version tag before merging: `git tag "skills/my-skill/v1.0.0" && git push origin "skills/my-skill/v1.0.0"`
4. Submit a PR — subsequent merges to `main` auto-increment patch versions
