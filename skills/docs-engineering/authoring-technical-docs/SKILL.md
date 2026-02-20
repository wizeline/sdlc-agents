---
name: authoring-technical-docs
description: "Core documentation engineering action. Provides the quality framework, style rules, and multi-pass workflow (research → draft → review → format) that all documentation must follow. Load this first for any technical documentation task. If no domain action matches the request, this action alone is sufficient."
context: fork
agent: Plan
---

# Authoring Technical Docs — Core Documentation Engineering Action

## Overview

This action defines the process, quality standards, and style rules for producing professional technical documentation. It follows a multi-pass workflow: research the inputs, draft the document, self-review for quality, and format for delivery.

**Always load this action first. Domain actions (authoring-api-docs, authoring-architecture-docs, authoring-release-docs, authoring-user-docs) build on this foundation.**

---

## Workflow

Execute these phases in order. Do not skip phases.

```
Request → RESEARCH → DRAFT → REVIEW → REVISE (max 2 cycles) → FORMAT → Deliver
```

---

## Phase 1: Research

Before writing a single word, consume every input artifact and build a structured understanding. Documentation is 80% research, 20% writing.

### What to extract

**From source code:** function/method signatures, class hierarchies, module structure, docstrings, inline comments, error types, dependencies.

**From OpenAPI / Swagger specs:** all endpoints with methods, paths, parameters, request/response schemas, auth requirements, rate limits.

**From PRDs / product specs:** user stories, acceptance criteria, target personas, feature scope and non-goals, business context.

**From existing documentation:** current structure and coverage, last-updated dates, terminology used, gaps relative to codebase.

**From Jira / ticket exports:** feature descriptions, bug descriptions, resolution status, linked PRs, labels.

### Audience identification

If not stated, infer from context:
- API spec + no UI docs → **developer**
- UI mockups + user stories → **end-user**
- Infrastructure code + deployment configs → **operator/admin**
- Architecture diagrams + design decisions → **internal engineering**

State the inference so the user can correct it.

### Gap detection (highest-value research activity)

Check for:
1. **Spec-code mismatches** — spec says one thing, code does another
2. **Missing information** — feature in PRD but no corresponding code
3. **Stale references** — docs reference things no longer in the code
4. **Undocumented behavior** — code handles edge cases no spec mentions
5. **Contradictions** — two sources disagree on the same fact

Classify each gap:
- **Blocker** — can't write accurate docs without resolution. Surface to user before drafting.
- **Warning** — can write around it, should verify
- **Info** — nice to know

### Documentation type (Diátaxis framework)

- **Tutorial** — audience is new, feature is complex, hands-on learning needed
- **How-To Guide** — audience knows basics, needs to accomplish a specific task
- **Explanation** — the "why" needs communicating, decisions need context
- **Reference** — precise technical details need cataloguing

Share a brief research summary before proceeding to drafting.

---

## Phase 2: Draft

Write the document following the style rules below and the domain action's templates.

### Non-negotiable style rules

| Rule | Do | Don't |
|------|-----|-------|
| Voice | "The API returns a JSON object" | "A JSON object is returned by the API" |
| Person | "You can configure..." | "The user can configure..." |
| Tense | "This endpoint accepts..." | "This endpoint will accept..." |
| Instructions | "Click **Save**." | "You should click Save." |
| Headings | "Set up your environment" (sentence case) | "Setting Up Your Environment" |
| Lists | Oxford comma: "logs, metrics, and traces" | "logs, metrics and traces" |
| Jargon | Define on first use: "the payload (the data sent in the request body)" | Undefined jargon |
| Sentences | One idea per sentence. Max ~25 words. | Long compound sentences |

### YAML frontmatter (every document)

```yaml
---
title: "[Document title]"
description: "[One-line description]"
audience: [developer | end-user | admin | operator]
doc-type: [tutorial | how-to | reference | explanation]
last-updated: [YYYY-MM-DD]
---
```

### Handling gaps

- **Blocker gaps**: `<!-- [GAP: description — needs human verification] -->`
- **Warning gaps**: Write best available version, add `<!-- [UNVERIFIED: description] -->`
- **Never invent facts.** A visible gap is better than a hidden error.

### Code examples

- Every API endpoint or function gets at least one working example
- Complete and runnable — no `...` or `// rest of code`
- Placeholder values clearly marked: `YOUR_API_KEY`, `<project-id>`
- Pair request examples with expected responses

---

## Phase 3: Review (self-critique)

After drafting, switch to critic mode. Apply the **First-Party Consumption Principle**: read the draft as the target audience encountering it for the first time. Ask: "Could I accomplish the goal using ONLY this document?"

### Six quality dimensions

1. **Accuracy** — every factual claim traceable to a source input
2. **Completeness** — no missing parameters, steps, or sections; prerequisites listed; error handling present
3. **Usability** — each procedure step is unambiguous; code examples are runnable
4. **Consistency** — same term used throughout; heading style uniform; code examples use same conventions
5. **Readability** — sentences ≤25 words; no passive voice; no undefined jargon
6. **Structure** — heading hierarchy sequential; info flows general → specific; code blocks have language tags

### Issue severity

- **Blocker**: factual error, missing prerequisite, broken code example, wrong audience, missing critical section
- **Major**: passive voice, missing error handling, undefined jargon, ambiguous pronoun, inconsistent terminology
- **Minor**: missing Oxford comma, title case heading, future tense, third person

If blockers or major issues found → revise. Maximum 2 revision cycles.

---

## Phase 4: Format and deliver

1. Fix heading hierarchy silently
2. Ensure YAML frontmatter is complete
3. Add language tags to untagged code blocks
4. Generate table of contents if 4+ H2 sections
5. Save to `docs/` relative to project root with a descriptive filename

### Unresolved issues

If any issues remain after revision cycles, append as editor's notes:

```markdown
---

## Editor's notes

- [Issue description and suggested fix]
```

---

## Guiding principles

1. **Documentation is 80% research, 20% writing.** Never start writing before understanding the full picture.
2. **Source of Truth Principle.** Every factual claim must trace to an input artifact.
3. **First-Party Consumption Principle.** Read your own docs as if you're a first-time user.
4. **Flag, don't guess.** Unclear → mark as gap, don't infer.
5. **Show, don't tell.** Code examples and concrete scenarios beat abstractions.
6. **Progressive disclosure.** Common path first, edge cases in variations.
7. **Every document ends somewhere.** Always provide next steps.
