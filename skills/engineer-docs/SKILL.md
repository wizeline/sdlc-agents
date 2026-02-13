---
name: engineer-docs
description: "A professional documentation engineering skill for software development projects. Use this skill whenever someone requests technical documentation—including API references, user guides, tutorials, how-to guides, release notes, READMEs, architecture docs, installation guides, or in-app help text. Also trigger when the user wants to document a codebase, generate docs from source code/OpenAPI specs, review existing docs for quality, create a documentation plan, or convert between formats. Trigger on phrases like 'document this', 'write docs', 'explain this code', or 'create a knowledge base'. DO NOT use for general creative writing, marketing copy, or non-software documentation."
---

# Engineer Docs — Documentation Engineering Skill

## Overview

This skill transforms the AI into a documentation engineering team that operates as an integrated part of the Software Development Life Cycle (SDLC). It combines two powerful paradigms:

- **Docs-as-Code**: Documentation lives alongside source code, uses version control, markup formats, and CI-style quality checks
- **Multi-Agent Orchestration**: Specialized subagents handle research, writing, criticism, and formatting through iterative refinement—mimicking professional documentation teams

## The Agent Pod

Four specialized agents execute a structured workflow. Each has dedicated instructions in `agents/`:

| Agent | File | Purpose | Execution |
|-------|------|---------|-----------|
| **Researcher** | `agents/researcher.md` | Input analysis, gap detection, audience identification, source-of-truth mapping | Runs once before drafting |
| **Writer** | `agents/writer.md` | Drafts from research brief using Diátaxis-appropriate templates | Runs initially + up to 2 revisions |
| **Critic** | `agents/critic.md` | Reviews for accuracy, completeness, usability via First-Party Consumption | Runs after each Writer pass |
| **Formatter** | `agents/formatter.md` | Structural validation, linting, format conversion | Final pass before delivery |

### Orchestration Workflow

```
User Request
     │
     ▼
┌─────────────┐
│  RESEARCHER │ ← Analyzes code, specs, PRDs; performs Gap Detection
└──────┬──────┘    Output: research_brief.md with inventory, audience, gaps
       │
       ▼
┌─────────────┐
│   WRITER    │ ← Drafts using Diátaxis template + style rules
└──────┬──────┘    Output: draft.md with YAML frontmatter
       │
       ▼
┌─────────────┐
│   CRITIC    │ ← First-Party Consumption review: "Could I complete this task with ONLY this doc?"
└──────┬──────┘    Output: review.md with severity-ranked issues
       │
   ┌───────┐
   │ Issues │──yes──► WRITER (revise) ──► CRITIC (re-review)
   │ found? │         (max 2 revision cycles)
   └───┬───┘
       │ no
       ▼
┌─────────────┐
│ FORMATTER   │ ← Final lint, TOC generation, format conversion
└──────┬──────┘
       │
       ▼
  Final Document (with editor's notes for unresolved issues)
```

## Key Methodologies

| Methodology | Application |
|-------------|-------------|
| **First-Party Consumption** | Critic reads draft as a novice user with ONLY the document—no prior knowledge |
| **Source of Truth Principle** | Every factual claim traces to a verifiable artifact (code, spec, PRD) |
| **Gap Detection Protocol** | Flags contradictions between PRDs, design mockups, and actual implementation |
| **Diátaxis Framework** | Matches content type to appropriate structure:<br>• Tutorial (learning-oriented)<br>• How-To Guide (task-oriented)<br>• Explanation (understanding-oriented)<br>• Reference (information-oriented) |

## When to Read Reference Files

Before starting work, consult these references based on task type:

| Task involves... | Read this reference |
|------------------|---------------------|
| API endpoints, SDKs, OpenAPI specs | `references/api_docs.md` |
| User guides, tutorials, how-to guides | `references/user_docs.md` |
| Release notes, changelogs, READMEs | `references/release_docs.md` |
| Architecture docs, design docs, ADRs | `references/architecture_docs.md` |
| SDLC phase alignment | `references/sdlc_workflow.md` |
| Quality rules, style, linting | `references/quality_framework.md` |
| Industry style standards | `references/style_guides.md` (Google, Microsoft) |

## Input Handling Guidance

| Input Type | How to use it |
|------------|---------------|
| Source code | Primary source of truth for behavior; extract signatures, types, error codes |
| OpenAPI/Swagger spec | Generate API reference structure directly from endpoints, parameters, schemas |
| PRD / Product spec | Extract "why" and audience context; user stories, acceptance criteria |
| Existing documentation | Analyze for gaps/outdated content; update rather than rewrite |
| Jira/ticket exports | Harvest feature descriptions, bug fixes for release notes |
| User feedback/support tickets | Identify pain points, FAQ candidates, confusion patterns |

## Non-Negotiable Style Rules

All output must follow these standards:

| Rule | Do | Don't |
|------|----|-------|
| **Voice** | "The API returns a JSON object" | "A JSON object is returned by the API" |
| **Person** | "You can configure..." | "The user can configure..." |
| **Tense** | "This endpoint accepts..." | "This endpoint will accept..." |
| **Instructions** | "Click Save" | "You should click Save" |
| **Headings** | Sentence case: "Set up your environment" | Title Case: "Setting Up Your Environment" |
| **Lists** | Oxford comma: "logs, metrics, and traces" | "logs, metrics and traces" |
| **Jargon/Acronyms** | Define on first use: "payload (data in request body)" | Use undefined terms |
| **Code examples** | Every endpoint/function has working example with syntax highlighting | Examples without context or validation |

## Output Format Defaults

Unless user specifies otherwise:
- All documentation types → Markdown (.md) with YAML frontmatter
- Documents >3 sections → auto-generated table of contents
- For .docx/.pdf/.html requests → invoke respective conversion skills after Markdown draft completion

## Quality Gate Checklist

Every delivered document must pass:

- [ ] **Accuracy** — All claims traceable to source artifact
- [ ] **Completeness** — No missing parameters, prerequisites, or error cases
- [ ] **Usability** — Target user can complete task using ONLY this document
- [ ] **Consistency** — Uniform terminology; no synonym drift
- [ ] **Readability** — Flesch-Kincaid ≤12 (user docs) or ≤16 (developer docs)
- [ ] **Structure** — Follows appropriate Diátaxis quadrant
- [ ] **Error handling** — Common failures and solutions documented
- [ ] **Link integrity** — No broken internal references

---

## Error handling

- **Subagent fails**: Retry once. If it fails again, execute the agent's instructions inline.
- **Skill files not found**: Fall back to standard conversion libraries.
- **No input files**: Ask what needs documenting. Offer to scan the project directory.
- **Ambiguous doc type**: Ask the user. Present Diátaxis options: Tutorial, How-To, Reference, Explanation.
