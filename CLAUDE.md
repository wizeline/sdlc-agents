# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Wize Skills is a monorepo of modular AI Cores built on the [Agent Skills open standard](https://agentskills.io/home). Each AI Core (in `aicores/`) contains its own set of specialized Agents and Skills. Skills are **documentation-based** — there is no application code, build step, or test suite.

## Repository Structure

```
aicores/
  {core-name}/
    skills/
      {skill-name}/
        SKILL.md          # Skill definition (YAML frontmatter + markdown)
        references/       # Domain-specific knowledge, templates, best practices
        assets/           # Reusable output templates
    agents/               # Agent orchestrators for this core
docs/                     # Design documents and tutorials
```

### Current AI Cores

- **documentation-writer-agent** — Multi-agent documentation pipeline (Researcher → Writer → Reviewer → Formatter) using the Diátaxis framework.
- **security-agent** — SDLC security guidance powered by OWASP Top 10 (2025), NIST SSDF, ASVS 5.0, and 12 reference frameworks.
- **unit-testing-agent** — Automated unit test generation, review, and coverage analysis.

## Key Architecture Concepts

**SKILL.md** is the entry point for each skill. It contains YAML frontmatter (`name`, `description`, `context`, `agent`) and markdown sections defining when the skill triggers, its core workflow, framework selection tables, and output format guidelines.

**Multi-agent orchestration**: Specialized agents in `aicores/{core-name}/agents/` handle specific phases. They communicate via intermediate markdown artifacts.

**Reference files** are static knowledge bases that skills route to via decision tables in SKILL.md.

## Versioning & CI

Each skill is independently versioned via git tags: `aicores/{core-name}/skills/{skill-name}/v{MAJOR}.{MINOR}.{PATCH}`

The GitHub Actions workflow (`.github/workflows/version-skills.yml`) auto-increments the **patch** version on every push to `main` that modifies files under `aicores/`.

For major/minor bumps, create tags manually:
```bash
git tag "aicores/documentation-writer-agent/skills/my-skill/v2.0.0"
git push origin "aicores/documentation-writer-agent/skills/my-skill/v2.0.0"
```

## Adding a New Skill

1. Identify the appropriate AI Core in `aicores/` (or create a new one).
2. Create `aicores/{core-name}/skills/my-skill/` with at minimum a `SKILL.md`.
3. Add `references/` and optionally `assets/` directories.
4. Seed the initial version tag: `git tag "aicores/{core-name}/skills/my-skill/v1.0.0" && git push origin --tags`
5. Submit a PR — subsequent merges to `main` auto-increment patch versions.

