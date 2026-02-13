---
name: docs-engineering
description: "A professional documentation engineering workflow for software development projects. Use this workflow whenever someone requests technical documentation—including API references, user guides, tutorials, how-to guides, release notes, READMEs, architecture docs, installation guides, or in-app help text. Also trigger when the user wants to document a codebase, generate docs from source code/OpenAPI specs, review existing docs for quality, create a documentation plan, or convert between formats. Trigger on phrases like 'document this', 'write docs', 'explain this code', or 'create a knowledge base'. DO NOT use for general creative writing, marketing copy, or non-software documentation."
context: fork
agent: Explore
---

# Docs Engineering Skill

## Overview

This skill implements a professional documentation engineering process that integrates with the Software Development Life Cycle (SDLC). It combines two powerful paradigms:

- **Docs-as-Code**: Documentation lives alongside source code, uses version control, markup formats, and CI-style quality checks
- **Multi-Agent Orchestration**: Specialized subagents handle research, writing, criticism, and formatting through iterative refinement—mimicking professional documentation teams

## The Agent Pod

Four specialized agents execute a structured workflow. Each has dedicated instructions in `agents/`:

| Phase | Reference File | Task Objectives | When to Execute |
|-------|----------------|-----------------|-----------------|
| **Research** | `agents/researcher.md` | Analyze inputs, detect gaps, identify audience, map sources of truth | Execute once before drafting |
| **Drafting** | `agents/writer.md` | Create content from research brief using Diátaxis-appropriate templates | Execute initially + up to 2 revision cycles |
| **Review** | `agents/reviewer.md` | Validate accuracy, completeness, usability via First-Party Consumption | Execute after each drafting pass |
| **Formatting** | `agents/formatter.md` | Perform structural validation, linting, format conversion | Execute once as final pass |

### Workflow

```
User Request
     │
     ▼
┌─────────────────────────────────────────────────────────┐
│ PHASE 1: RESEARCH                                       │
│ • Analyze code, specs, PRDs                             │
│ • Perform Gap Detection                                 │
│ • Identify target audience                              │
│ • Map sources of truth                                  │
└──────┬──────────────────────────────────────────────────┘
       │ Output: research_brief.md with inventory, audience, gaps
       ▼
┌─────────────────────────────────────────────────────────┐
│ PHASE 2: DRAFTING                                       │
│ • Select appropriate Diátaxis template                  │
│ • Apply style rules from references                     │
│ • Create content sections                               │
│ • Add code examples with syntax highlighting            │
└──────┬──────────────────────────────────────────────────┘
       │ Output: draft.md with YAML frontmatter
       ▼
┌─────────────────────────────────────────────────────────┐
│ PHASE 3: REVIEW                                         │
│ • Execute First-Party Consumption test                  │
│ • Check: "Could I complete this task with ONLY this?"   │
│ • Rank issues by severity (blocking/major/minor)        │
│ • Document gaps and inaccuracies                        │
└──────┬──────────────────────────────────────────────────┘
       │ Output: review.md with severity-ranked issues
       │
   ┌───────────────┐
   │ Issues found? │──YES──► Return to PHASE 2 (max 2 cycles)
   └───┬───────────┘
       │ NO
       ▼
┌─────────────────────────────────────────────────────────┐
│ PHASE 4: FORMATTING                                     │
│ • Run linting checks                                    │
│ • Generate table of contents                            │
│ • Convert to requested format                           │
│ • Add editor's notes for unresolved issues              │
└──────┬──────────────────────────────────────────────────┘
       │
       ▼
  Final Document
```

## Key Methodologies to Apply

| Methodology | How to Apply It |
|-------------|-----------------|
| **First-Party Consumption** | Read the draft as a novice user with ONLY the document—no prior knowledge. Ask: "Can I complete the task with just this?" |
| **Source of Truth Principle** | Trace every factual claim to a verifiable artifact (code, spec, PRD). Never make assumptions. |
| **Gap Detection Protocol** | Compare PRDs, design mockups, and actual implementation. Flag contradictions explicitly. |
| **Diátaxis Framework** | Match content type to structure:<br>• **Tutorial** (learning-oriented): Step-by-step lessons<br>• **How-To Guide** (task-oriented): Problem-solving recipes<br>• **Explanation** (understanding-oriented): Conceptual clarification<br>• **Reference** (information-oriented): Technical descriptions |

## Pre-Work: Read Reference Files

Before executing each phase, consult these references based on task type:

| Task involves... | Read this reference first |
|------------------|---------------------------|
| API endpoints, SDKs, OpenAPI specs | `references/api_docs.md` |
| User guides, tutorials, how-to guides | `references/user_docs.md` |
| Release notes, changelogs, READMEs | `references/release_docs.md` |
| Architecture docs, design docs, ADRs | `references/architecture_docs.md` |
| SDLC phase alignment | `references/sdlc_workflow.md` |
| Quality rules, style, linting | `references/quality_framework.md` |
| Industry style standards | `references/style_guides.md` (Google, Microsoft) |

## Input Handling Guidance

| Input Type | Extraction Strategy |
|------------|---------------------|
| Source code | Extract signatures, types, error codes as primary source of truth for behavior |
| OpenAPI/Swagger spec | Generate API reference structure directly from endpoints, parameters, schemas |
| PRD / Product spec | Extract "why" and audience context; harvest user stories, acceptance criteria |
| Existing documentation | Analyze for gaps/outdated content; update rather than rewrite from scratch |
| Jira/ticket exports | Harvest feature descriptions, bug fixes for release notes |
| User feedback/support tickets | Identify pain points, FAQ candidates, confusion patterns |

## Non-Negotiable Style Rules

Apply these standards to all output:

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

## Error Handling Procedures

| Error Condition | Recovery Steps |
|-----------------|----------------|
| Phase execution fails | Retry once. If second failure, execute the phase's reference instructions inline. |
| Reference files not found | Fall back to standard conversion libraries and industry best practices. |
| No input files provided | Ask what needs documenting. Offer to scan the project directory. |
| Ambiguous doc type | Ask the user. Present Diátaxis options: Tutorial, How-To, Reference, Explanation. |
