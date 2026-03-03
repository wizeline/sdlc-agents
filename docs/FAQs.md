---
title: "Wize Skills — Frequently Asked Questions"
description: "Answers to common questions about installing, customizing, and using Wize Skills across AI coding assistants."
doc-type: reference
last-updated: 2026-02-27
---

# Wize Skills — Frequently Asked Questions

## Table of Contents

### General

1. [My team already uses Cursor for documentation. Why is this different, and why is it better?](#1-my-team-already-uses-cursor-for-documentation-why-is-this-different-and-why-is-it-better)
2. [How do I customize a skill for a specific client?](#2-how-do-i-customize-a-skill-for-a-specific-client)
3. [How do I reinstall skills if they're already installed?](#3-how-do-i-reinstall-skills-if-theyre-already-installed)
4. [Do I need a specific Git account to install Wize Skills?](#4-do-i-need-a-specific-git-account-to-install-wize-skills)
5. [How do I install and use Wize Skills in Cursor?](#5-how-do-i-install-and-use-wize-skills-in-cursor)

### Documentation Writer Agent

6. [What agents and skills are included in the Documentation Writer AI Core?](#6-what-agents-and-skills-are-included-in-the-documentation-writer-ai-core)
7. [What types of documents can the Documentation Writer produce?](#7-what-types-of-documents-can-the-documentation-writer-produce)
8. [What is the Diátaxis framework and how does the skill use it?](#8-what-is-the-diataxis-framework-and-how-does-the-skill-use-it)
9. [How does the multi-pass workflow work?](#9-how-does-the-multi-pass-workflow-work)
10. [What inputs does the agent accept?](#10-what-inputs-does-the-agent-accept)
11. [What output formats are supported beyond Markdown?](#11-what-output-formats-are-supported-beyond-markdown)
12. [How does the self-review work, and what are the six quality dimensions?](#12-how-does-the-self-review-work-and-what-are-the-six-quality-dimensions)
13. [Can the agent update documentation automatically when I commit code?](#13-can-the-agent-update-documentation-automatically-when-i-commit-code)
14. [How do I choose between a tutorial, how-to guide, user guide, and onboarding guide?](#14-how-do-i-choose-between-a-tutorial-how-to-guide-user-guide-and-onboarding-guide)
15. [What templates are available, and where are documents saved?](#15-what-templates-are-available-and-where-are-documents-saved)

### Security Agent

16. [What agents and skills are included in the Security AI Core?](#16-what-agents-and-skills-are-included-in-the-security-ai-core)
17. [Which security agent should I use for my task?](#17-which-security-agent-should-i-use-for-my-task)
18. [What is a Real-Time Report, and how is it different from a Remediation Guide?](#18-what-is-a-real-time-report-and-how-is-it-different-from-a-remediation-guide)
19. [What is a Vulnerability Map?](#19-what-is-a-vulnerability-map)
20. [What is a Compliance Log?](#20-what-is-a-compliance-log)
21. [Which security frameworks and standards are covered?](#21-which-security-frameworks-and-standards-are-covered)
22. [Does the Security Agent support AI and LLM security?](#22-does-the-security-agent-support-ai-and-llm-security)
23. [Which CI/CD platforms are supported for pipeline hardening?](#23-which-cicd-platforms-are-supported-for-pipeline-hardening)
24. [Can the agent scan for deprecated or end-of-life libraries?](#24-can-the-agent-scan-for-deprecated-or-end-of-life-libraries)
25. [When should I use threat modeling vs. a code review?](#25-when-should-i-use-threat-modeling-vs-a-code-review)
26. [What is "write once, comply many"?](#26-what-is-write-once-comply-many)
27. [How do I run an OWASP SAMM maturity assessment?](#27-how-do-i-run-an-owasp-samm-maturity-assessment)

---

## 1. My team already uses Cursor for documentation. Why is this different, and why is it better?

**Short answer:** Using Cursor without skills gives you a general-purpose AI assistant. Wize Skills gives that same AI structured, Wizeline-specific expertise, a defined workflow, and curated templates — so the output is consistent, reviewable, and repeatable.

### Cursor's default AI vs. Wize Skills

| | **Cursor (no skills)** | **Wize Skills** |
|---|---|---|
| Knowledge | General-purpose, trained on public data | Domain-specific: Diátaxis, OWASP Top 10 (2025), NIST SSDF, ASVS 5.0, and more |
| Workflow | Unstructured — each session starts from scratch | Structured multi-phase pipeline: Research → Draft → Review → Format |
| Output quality | Varies by how well the prompt is written | Consistent — the skill enforces style rules, gap detection, and self-review |
| Templates | None built in | Client-ready templates for API docs, architecture docs, release notes, user guides |
| Tool compatibility | Cursor only | Claude Code, Cursor, Gemini CLI, OpenAI Codex — one skill works across all |
| Versioning | N/A | Independently versioned per skill; teams pull specific versions |
| Sharing | Must re-explain everything in each session | Skills are committed to git and shared across the team automatically |

### What skills actually change

When a Wize Skill activates, the AI agent gains:

- **A procedural workflow** it must follow (no skipping phases, no guessing).
- **Reference knowledge** — for example, the `dev-security-governance` skill knows every OWASP Top 10 category and maps them to your codebase automatically.
- **Templates and assets** — ready-to-fill documents so output structure is consistent across engineers.
- **Self-review criteria** — the agent critiques its own output before delivering it, catching accuracy, completeness, and style issues.

Cursor's built-in AI can do many of the same things if you write a detailed prompt every time. Skills move that expertise out of your prompt and into a reusable, version-controlled package.

---

## 2. How do I customize a skill for a specific client?

Skills are designed to be forked and adapted. The three main customization points are **templates**, **reference knowledge**, and **workflow instructions**.

### Customize templates (`assets/`)

Each skill's `assets/` directory contains the document templates the AI fills in. Replace or modify these to match the client's brand, format, or content requirements.

**Example — replace the API doc template:**

```
skills/docs-engineering/authoring-api-docs/assets/rest-endpoint-template.md
```

Open the file and update sections to reflect the client's conventions: header formats, required fields, terminology, etc. The AI will use this file whenever it generates an API doc for that skill.

### Customize reference knowledge (`references/`)

The `references/` directory holds domain knowledge the skill draws on — coding standards, security frameworks, notation guides. Add a client-specific reference file to extend what the skill knows.

**Example — add a client coding standard:**

1. Create `skills/dev-security-governance/references/acme-secure-coding-guide.md`.
2. Add the client's policies and naming conventions to this file.
3. In `SKILL.md`, update the routing table to point relevant queries at the new reference.

### Customize workflow instructions (`SKILL.md`)

The `SKILL.md` file defines the AI's behavior: when to activate, what steps to follow, what rules to enforce. Edit the markdown body directly to add or remove steps, change output format, or tighten quality checks for a specific client.

**Example — add a mandatory client sign-off step:**

```markdown
## Phase 4: Format and deliver

...existing steps...

5. Append the following sign-off block to every document before delivering:

   > **Pending review by:** [Client name] Documentation Lead
```

### Keep customizations isolated

If you are managing skills for multiple clients, maintain a separate branch or fork per client rather than modifying the shared `main` branch. This prevents one client's customizations from leaking into another engagement.

---

## 3. How do I reinstall skills if they're already installed?

If a skills directory already exists from a previous installation, the installer may fail or merge incorrectly. Delete the existing directory first.

### Find the existing skills directory

The location depends on which agent tool you are using and whether the install was scoped to the project or the user.

| Agent | Project scope | User scope |
|---|---|---|
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Cursor | `.cursor/skills/` | `~/.cursor/skills/` |
| Gemini CLI | `.agents/skills/` or `.gemini/skills/` | `~/.agents/skills/` or `~/.gemini/skills/` |
| OpenAI Codex | `.codex/skills/` | `~/.codex/skills/` |

### Delete the skill or agent directory and reinstall

```bash
# Example: removing and reinstalling the docs-engineering skill for Claude Code (project scope)
rm -rf .claude/skills/docs-engineering

# Reinstall the skill
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a claude-code

# Example: removing and reinstalling the doc-engineer subagent
rm -rf .claude/agents/doc-engineer

# Reinstall the subagent
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer -a claude-code
```

> **Note:** If you customized any templates or references before deleting, back them up first. The reinstall overwrites the directory completely.

---

## 4. Do I need a specific Git account to install Wize Skills?

Yes. The `wize-skills` repository is hosted in the Wizeline GitHub organization. You need a GitHub account with access to that organization to clone or install from it.

### Configure Git with your Wizeline account

Before running any install command, make sure Git is configured with your Wizeline credentials:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.name@wizeline.com"
```

### Authenticate with GitHub

The `npx skills add` installer and the `gemini skills install` command both use Git under the hood. You need one of the following to authenticate:

**Option A — SSH key (recommended)**

1. Generate an SSH key if you don't have one:
   ```bash
   ssh-keygen -t ed25519 -C "your.name@wizeline.com"
   ```
2. Add the public key to your GitHub account under **Settings → SSH and GPG keys**.
3. Verify access:
   ```bash
   ssh -T git@github.com
   ```

**Option B — Personal Access Token (HTTPS)**

1. In GitHub, go to **Settings → Developer settings → Personal access tokens → Fine-grained tokens**.
2. Create a token with `Contents: Read` access to the `wizeline/wize-skills` repository.
3. When Git prompts for credentials, use your Wizeline email as the username and the token as the password. You can also store it with the credential helper:
   ```bash
   git config --global credential.helper store
   ```

---

## 5. How do I install and use Wize Skills in Cursor?

Cursor supports the [Agent Skills open standard](https://agentskills.io/home) natively. The official documentation covers the full setup:

- **Skills in Cursor:** https://cursor.com/docs/context/skills
- **Subagents in Cursor:** https://cursor.com/docs/context/subagents

### Install a skill

```bash
# Using the agents-skills CLI
npx skills add https://github.com/wizeline/wize-skills/tree/main/skills/docs-engineering -a cursor

# Or clone manually
git clone https://github.com/wizeline/wize-skills.git /tmp/wize-skills
cp -r /tmp/wize-skills/skills/docs-engineering .cursor/skills/
```

### Install a subagent

```bash
# Install a single subagent
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting/doc-engineer -a cursor

# Install an entire agent group
npx agents add https://github.com/wizeline/wize-skills/tree/main/agents/documenting -a cursor
```

Cursor discovers skills from:

| Scope | Path |
|---|---|
| Project (shared with team) | `.cursor/skills/` |
| User (personal, available everywhere) | `~/.cursor/skills/` |

### Activate a skill

Skills activate automatically when your request matches the skill's description. You can also invoke any skill directly with a slash command:

```
/docs-engineering
/dev-security-governance
```

### Use subagents

Cursor supports subagents for delegating complex tasks to isolated, specialized agents. Place subagent definition files in:

| Scope | Path |
|---|---|
| Project | `.cursor/agents/` |
| User | `~/.cursor/agents/` |

Refer to the [Cursor subagents documentation](https://cursor.com/docs/context/subagents) for the full frontmatter reference and configuration options.

### Verify the skill loaded

After installation, open Cursor and type `/` in the chat input. You should see the installed skills listed in the command menu. If a skill is missing, check that the `SKILL.md` file is present in the correct directory and that the YAML frontmatter contains a valid `name` field.

---

## Documentation Writer Agent

---

## 6. What agents and skills are included in the Documentation Writer AI Core?

The Documentation Writer AI Core includes **2 agents** backed by **9 skills**.

| Agent | Role | Skills used |
|---|---|---|
| `doc-engineer` | Full documentation pipeline — research, draft, review, format, and export | `authoring-technical-docs`, `authoring-api-docs`, `authoring-architecture-docs`, `authoring-release-docs`, `authoring-user-docs`, `editing-docx-files`, `editing-pptx-files`, `automating-docs-updates` |
| `c4-architect` | Specialized C4 Model diagram generation | `authoring-architecture-docs` |

**`doc-engineer`** is the primary agent for almost all documentation tasks. Use it directly for any writing, review, or format conversion work.

**`c4-architect`** is a focused subagent for architecture visualization. Use it when you need C4 Context, Container, Component, or Dynamic diagrams and want a dedicated agent with no other concerns.

---

## 7. What types of documents can the Documentation Writer produce?

The Documentation Writer covers every major category of technical documentation:

| Category | What it produces | Skill |
|---|---|---|
| **API reference** | REST endpoint docs, SDK references, CLI references, OpenAPI-to-human-readable conversion | `authoring-api-docs` |
| **Architecture docs** | Architecture Decision Records (ADRs), design documents, system overviews, C4 diagrams (Context/Container/Component/Dynamic) | `authoring-architecture-docs` |
| **Release docs** | Release notes, changelogs, READMEs, migration guides | `authoring-release-docs` |
| **User docs** | Tutorials, how-to guides, user guides, onboarding guides | `authoring-user-docs` |
| **General technical docs** | Technical specs, engineering proposals, doc audits, content reviews | `authoring-technical-docs` |
| **Word documents** | `.docx` files with redlining, comments, and table-of-contents generation | `editing-docx-files` |
| **Presentations** | `.pptx` files built from outlines or existing content | `editing-pptx-files` |
| **Automated doc updates** | Docs staged alongside code commits | `automating-docs-updates` |

For general documentation needs with no specific domain, `authoring-technical-docs` alone is sufficient. Domain skills (`authoring-api-docs`, `authoring-architecture-docs`, etc.) extend it with templates and domain-specific rules.

---

## 8. What is the Diátaxis framework and how does the skill use it?

[Diátaxis](https://diataxis.fr) is a documentation framework that classifies every technical document into one of four types based on what the reader needs:

| Type | Reader need | Skill |
|---|---|---|
| **Tutorial** | Learning — the reader is new and needs a guided experience | `authoring-user-docs` |
| **How-To Guide** | Task — the reader knows the basics and needs to accomplish something specific | `authoring-user-docs` |
| **Reference** | Information — the reader needs precise technical details | `authoring-api-docs` |
| **Explanation** | Understanding — the reader needs context, reasoning, and the "why" | `authoring-architecture-docs` |

The agent uses this classification to select the correct template, writing style, and structure for each document. If you don't specify a document type, the agent infers it from your request — for example, "document this endpoint" maps to Reference, while "write a guide for new developers" maps to Tutorial.

---

## 9. How does the multi-pass workflow work?

Every documentation task follows a four-phase pipeline. The agent cannot skip phases.

```
Request → RESEARCH → DRAFT → REVIEW → REVISE (max 2 cycles) → FORMAT → Deliver
```

**Phase 1 — Research:** The agent reads every input you provide (code, specs, tickets, existing docs) before writing a single word. It extracts facts, identifies gaps, infers the target audience, and classifies each gap as Blocker (must resolve before drafting), Warning (can write around), or Info. It shares a brief research summary before proceeding.

**Phase 2 — Draft:** The agent writes the document using strict style rules: active voice, second person ("you"), present tense, sentence-case headings, Oxford commas, sentences ≤25 words, and no undefined jargon. Every code example is complete and runnable.

**Phase 3 — Review (self-critique):** The agent reads its own draft as if it were the target audience encountering it for the first time — the *First-Party Consumption Principle*. It checks six quality dimensions (see [FAQ 12](#12-how-does-the-self-review-work-and-what-are-the-six-quality-dimensions)) and revises if it finds blockers or major issues. Maximum two revision cycles.

**Phase 4 — Format and deliver:** The agent fixes heading hierarchy, completes YAML frontmatter, adds language tags to code blocks, generates a table of contents for documents with four or more H2 sections, and saves the file to the correct path.

---

## 10. What inputs does the agent accept?

The agent can work from any combination of these inputs:

| Input type | Examples |
|---|---|
| Source code | Functions, route handlers, classes, modules — the agent extracts signatures, docstrings, and behavior |
| API specs | OpenAPI/Swagger `.yaml` or `.json` files — the agent parses all paths, parameters, schemas, and security requirements |
| Jira tickets / exports | Feature descriptions, bug reports, acceptance criteria — used for release notes and technical specs |
| Existing documentation | Current docs the agent should audit, update, or build on |
| PRDs / product specs | User stories, feature scope, acceptance criteria, personas |
| Architecture diagrams | Described in text or as existing diagram files |
| Raw notes | Engineering notes, meeting summaries, bullet-point outlines |

Provide as much context as possible. The agent explicitly follows a **Source of Truth Principle** — every factual claim it writes must trace back to something you provided. If it encounters a gap it cannot resolve from your inputs, it flags it with a visible marker rather than guessing.

---

## 11. What output formats are supported beyond Markdown?

The default output format is Markdown. The following additional formats are supported:

| Format | How to request | Skill |
|---|---|---|
| **Word (`.docx`)** | "Export this as a Word document" or "Create a memo in `.docx`" | `editing-docx-files` |
| **PowerPoint (`.pptx`)** | "Create a slide deck from these bullet points" | `editing-pptx-files` |
| **Mermaid diagrams** | Produced automatically within Markdown for architecture docs | `authoring-architecture-docs` |

Diagram files are saved as separate `.mermaid` files alongside the main `ARCHITECTURE.md` document, so they can be version-controlled and rendered in any Mermaid-compatible viewer (GitHub, Notion, VS Code extension, etc.).

---

## 12. How does the self-review work, and what are the six quality dimensions?

After drafting, the agent switches to critic mode and evaluates the document across six dimensions:

| Dimension | What it checks |
|---|---|
| **Accuracy** | Every factual claim is traceable to a source input — nothing invented |
| **Completeness** | No missing parameters, steps, prerequisites, or error handling |
| **Usability** | Each procedure step is unambiguous; code examples are runnable as written |
| **Consistency** | Same term used throughout; heading style uniform; code examples use the same conventions |
| **Readability** | Sentences ≤25 words; no passive voice; no undefined jargon |
| **Structure** | Heading hierarchy is sequential; information flows general → specific; code blocks have language tags |

Each issue found is classified by severity:

- **Blocker** — factual error, missing prerequisite, broken code example, missing critical section → the agent revises before delivering
- **Major** — passive voice, undefined jargon, inconsistent terminology, ambiguous pronoun → the agent revises if time allows (within 2 cycles)
- **Minor** — missing Oxford comma, title-case heading, future tense → flagged but does not block delivery

If blockers or major issues remain after two revision cycles, the agent appends an **Editor's Notes** section listing the unresolved items so you know exactly what still needs human attention.

---

## 13. Can the agent update documentation automatically when I commit code?

Yes. The `automating-docs-updates` skill is designed for this workflow. When you are about to commit a code change, you can prompt the agent to:

1. Identify which documentation files are affected by the change.
2. Update those documents to reflect the new behavior.
3. Stage the updated documentation files so they can be committed together with the code.

**Example prompt:**

```text
I am about to commit these changes to the user-service. Please update the
affected API documentation and architecture overviews, and stage the docs
so they can be committed together.
```

This keeps documentation in sync with code at the commit level rather than as a separate, periodic effort.

---

## 14. How do I choose between a tutorial, how-to guide, user guide, and onboarding guide?

Use this table to pick the right document type before prompting:

| Type | Audience | Goal | Structure |
|---|---|---|---|
| **Tutorial** | Beginners — the reader has never used this before | "I learned how this works" — hands-on, guided experience | Linear journey with a tangible win early; validation step at the end |
| **How-To Guide** | Users who know the basics | "I accomplished my task" — task completion, not teaching | Action-first steps; minimal theory; direct imperative mood |
| **User Guide** | All experience levels | "I understand the whole product" — comprehensive reference | Chapter-style; organized by what users want to achieve, not by UI layout |
| **Onboarding Guide** | New team members or developers joining a project | "I am ready to contribute" — environment to first PR | Sequential, milestone-based; role-specific paths; checklist at each phase |

If you are unsure, describe your audience and goal in the prompt. The agent will select the appropriate type and explain its reasoning before proceeding.

---

## 15. What templates are available, and where are documents saved?

### Available templates

| Template file | Used for |
|---|---|
| `authoring-api-docs/assets/rest-endpoint-template.md` | Individual REST API endpoints |
| `authoring-api-docs/assets/sdk-function-template.md` | SDK or library functions |
| `authoring-architecture-docs/assets/Architecture_Decision_Record_(ADR)_template.md` | Architecture Decision Records |
| `authoring-architecture-docs/assets/Design_document_template.md` | Technical design documents |
| `authoring-architecture-docs/assets/System_architecture_template.md` | System architecture overviews |
| `authoring-architecture-docs/assets/diagram-templates.md` | Per-level C4 diagram scaffolding |
| `authoring-release-docs/assets/release_notes_template.md` | Release notes |
| `authoring-user-docs/assets/tutorial_template.md` | Tutorials |
| `authoring-user-docs/assets/how_to_guide_template.md` | How-to guides |
| `authoring-user-docs/assets/user_guide_template.md` | User guides |
| `authoring-user-docs/assets/onboarding_guide_template.md` | Onboarding guides |

### Default save locations

| Document type | Default path |
|---|---|
| API reference | `docs/api/` |
| Architecture overview | `docs/architecture/ARCHITECTURE.md` |
| C4 diagrams | `docs/architecture/diagrams/*.mermaid` |
| Release notes | `docs/releases/v[version].md` |
| Changelog | `CHANGELOG.md` (project root) |
| README | `README.md` (project root) |
| Tutorials | `docs/guides/tutorial-*.md` |
| How-to guides | `docs/guides/how-to-*.md` |
| User guides | `docs/guides/guide-*.md` |
| Onboarding guides | `docs/guides/onboarding-*.md` |

You can override the save path by specifying it in your prompt.

---

## Security Agent

---

## 16. What agents and skills are included in the Security AI Core?

The Security AI Core includes **6 agents**, each backed by a dedicated skill targeting a specific security persona and task.

| Agent | Persona | Skill |
|---|---|---|
| `devsec-code-review` | Developer reviewing or writing code | `reviewing-code-for-security` |
| `devsec-threat-modeling` | Architect at the design phase | `conducting-threat-modeling` |
| `devsec-architecture` | Architect designing APIs, cloud-native, or AI systems | `designing-security-architecture` |
| `devsec-ops-pipeline` | DevOps / platform engineer | `hardening-devsecops-pipelines` |
| `devsec-compliance-framework` | Compliance / GRC professional, pre-audit | `managing-compliance-frameworks` |
| `devsec-program` | AppSec lead or CISO building a program | `building-security-programs` + `managing-compliance-frameworks` |

Each agent loads only the reference material relevant to its domain. This keeps context lean and routing precise — "review this code for XSS" activates `devsec-code-review` rather than loading all six skills at once.

---

## 17. Which security agent should I use for my task?

Use this decision guide:

| If you want to… | Use this agent |
|---|---|
| Review existing code for vulnerabilities (SQLi, XSS, CSRF, injection, etc.) | `devsec-code-review` |
| Get a secure code checklist for your language/framework | `devsec-code-review` |
| Identify threats to a design *before* writing code | `devsec-threat-modeling` |
| Understand what your attack surface looks like | `devsec-threat-modeling` |
| Secure a REST API, GraphQL API, or microservices architecture | `devsec-architecture` |
| Set up OAuth 2.0, JWT, mTLS, or zero-trust patterns | `devsec-architecture` |
| Assess or secure an LLM/AI application | `devsec-architecture` |
| Add SAST, SCA, DAST, or secret scanning to your CI/CD pipeline | `devsec-ops-pipeline` |
| Generate an SBOM or harden your supply chain | `devsec-ops-pipeline` |
| Map your controls to ISO 27001, SOC 2, PCI-DSS, HIPAA, or NIST SSDF | `devsec-compliance-framework` |
| Prepare audit evidence or track security KPIs | `devsec-compliance-framework` |
| Assess your organization's security maturity (OWASP SAMM) | `devsec-program` |
| Build a Security Champions program or a 12-month AppSec roadmap | `devsec-program` |

For tasks that span multiple domains — for example, "threat model this API design, then review the code for those threats, then document the findings as an ADR" — you can chain agents in a single prompt. Each agent activates in sequence.

---

## 18. What is a Real-Time Report, and how is it different from a Remediation Guide?

Both are named output artifacts produced by the `devsec-code-review` agent. They serve different purposes.

**Real-Time Report** — the output of a code review. It:

- Lists every finding in priority order: Critical → High → Medium → Low
- Maps each finding to an OWASP Top 10 (2025) category and a CWE ID
- Maps each finding to the corresponding ASVS 5.0 requirement
- Shows before/after code fixes in the user's actual language, framework, and variable names
- Notes which findings could be caught by SAST tools (with specific rule names)
- Includes a full OWASP Top 10 coverage table showing which categories were checked, even when no finding was present for that category

**Remediation Guide** — the output when you ask *how to fix* a specific finding. It:

- Covers one finding at a time
- Follows five mandatory steps: Scope → Fix → Test → Prevent Recurrence → Deploy & Verify
- Provides both a functional test and a security test that confirms the attack vector is closed
- Documents a verification sign-off that can be entered into a Compliance Log as evidence

**When to use each:**

```text
# Request a Real-Time Report
"Review this Express.js auth middleware for security issues. Target ASVS Level 2."

# Request a Remediation Guide
"Generate a Remediation Guide for the SQL injection finding in our order service."
```

---

## 19. What is a Vulnerability Map?

A Vulnerability Map is a structured artifact produced by the `devsec-threat-modeling` agent. It complements the Threat Model Document by providing an actionable, scannable view that developers and security teams use for triage.

A complete Vulnerability Map contains:

| Section | What it shows |
|---|---|
| **Attack Surface Inventory** | Every external and internal entry point with protocol, authentication mechanism, and trust level |
| **Trust Boundary Map** | Visual or ASCII diagram of trust zones and data flows |
| **STRIDE Threat Inventory** | Per-component table mapping each STRIDE threat (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) to likelihood, impact, risk rating, and specific mitigation |
| **Deprecated & Vulnerable Library Risks** | Known-vulnerable or end-of-life dependencies with CVE reference, CVSS score, and recommended action — maps to OWASP A06:2025 (Vulnerable and Outdated Components) |
| **Risk Register** | Prioritized list with assigned owner and remediation timeline |

The Threat Model Document provides the reasoning and derived security requirements; the Vulnerability Map provides the actionable triage list.

---

## 20. What is a Compliance Log?

A Compliance Log is an audit-ready ledger produced by the `devsec-compliance-framework` agent. It consolidates all security control evidence into a single document structured for auditors.

A complete Compliance Log contains:

| Section | Purpose |
|---|---|
| **Control Activity Log** | Every security control event (scans, pen tests, training, access reviews) with evidence link and mapping to every applicable framework in a single row |
| **Vulnerability & Finding Log** | Every finding from SAST/DAST/SCA/pen tests with SLA target, actual remediation date, and evidence — used to calculate SLA compliance rates |
| **Security Metrics Dashboard** | MTTD, MTTR, and coverage KPIs tracked against defined targets |
| **Framework Compliance Status** | Pass/gap count per framework (ISO 27001, SOC 2, PCI-DSS, NIST SSDF, ASVS, GDPR) in a summary table |
| **Evidence Index** | Every piece of evidence cited in the Control Activity Log, with retention date and framework mapping |

The Compliance Log implements the **"write once, comply many"** principle (see [FAQ 26](#26-what-is-write-once-comply-many)): a single piece of evidence — for example, a SAST scan report — maps to all applicable framework requirements in one row.

---

## 21. Which security frameworks and standards are covered?

The Security AI Core references the following frameworks and standards across its six skills:

| Framework / Standard | Used by |
|---|---|
| **OWASP Top 10 (2025)** | All skills — primary risk categorization |
| **OWASP API Security Top 10** | `designing-security-architecture` |
| **OWASP LLM Top 10 (2025)** | `designing-security-architecture` |
| **OWASP SAMM (Software Assurance Maturity Model)** | `building-security-programs` |
| **ASVS 5.0 (Application Security Verification Standard)** | `reviewing-code-for-security`, `managing-compliance-frameworks` |
| **NIST SSDF 1.1 (Secure Software Development Framework)** | `building-security-programs`, `managing-compliance-frameworks` |
| **ISO 27001:2022** | `managing-compliance-frameworks` |
| **SOC 2** | `managing-compliance-frameworks` |
| **PCI-DSS** | `managing-compliance-frameworks` |
| **HIPAA** | `managing-compliance-frameworks` |
| **GDPR (Article 32)** | `managing-compliance-frameworks` |
| **CIS Benchmarks** | `managing-compliance-frameworks` |
| **SLSA (Supply-chain Levels for Software Artifacts)** | `hardening-devsecops-pipelines` |
| **STRIDE** | `conducting-threat-modeling` |
| **CWE (Common Weakness Enumeration)** | `reviewing-code-for-security` |

Every code review finding is mapped to at least one OWASP Top 10 category, one CWE ID, and one ASVS 5.0 requirement. Every compliance output cites the specific framework version being referenced (e.g., "ISO 27001:2022", "NIST SSDF 1.1").

---

## 22. Does the Security Agent support AI and LLM security?

Yes. The `devsec-architecture` agent includes dedicated coverage for AI and LLM application security via the `designing-security-architecture` skill, which references the **OWASP LLM Top 10 (2025)**.

Key areas covered:

| Risk | Coverage |
|---|---|
| **Prompt Injection (LLM01)** | Architectural separation of system instructions from user input; output validation before tool invocation |
| **Sensitive Information Disclosure (LLM02)** | Output filtering, data classification before feeding into prompts |
| **Supply Chain & Model Security (LLM03)** | Model version pinning, provenance verification, third-party plugin auditing, fine-tuning dataset scanning |
| **Agentic System Security (LLM08)** | Minimal privilege for agent tool calls; human-in-the-loop for irreversible actions; full audit trail of tool calls |
| **RAG / Knowledge Base Security** | Access control on retrieved documents; content sanitization before prompt injection; tenant isolation in vector stores |
| **Multi-tenant Data Leakage** | Design patterns for isolating user data in shared LLM pipelines |

**Example prompts:**

```text
How do I prevent prompt injection in our customer-facing LLM chatbot? It uses RAG with a vector database.

Review our agentic AI system that uses LangChain to call external APIs.
What risks exist regarding data leakage between tenants?
```

---

## 23. Which CI/CD platforms are supported for pipeline hardening?

The `devsec-ops-pipeline` agent provides ready-to-paste configuration for the following platforms:

| Platform | Supported |
|---|---|
| **GitHub Actions** | Full support — SAST, SCA, secret scanning, container scanning, SBOM, security gates |
| **GitLab CI** | Full support — same tooling categories |
| **Jenkins** | Supported |
| **CircleCI** | Supported |
| **Azure DevOps** | Supported |

For each platform, the agent produces:

- The actual YAML or pipeline configuration snippet, ready to paste into the target platform
- Performance expectations (estimated scan time, resource cost)
- Scope limits (what the gate will and won't catch)
- Tuning tips to reduce false positives before enforcing hard-fail thresholds

The agent also supports pre-commit hooks (via Gitleaks, TruffleHog) for secret scanning and IDE-level linting. The recommended rollout is to start in warn mode and move to hard-fail only after establishing a false-positive baseline.

---

## 24. Can the agent scan for deprecated or end-of-life libraries?

Yes. Deprecated and end-of-life (EOL) component detection is built into three skills:

| Skill | How it handles deprecated/EOL components |
|---|---|
| `reviewing-code-for-security` | Identifies EOL or deprecated libraries as a code review finding; maps to OWASP A06:2025 (Vulnerable and Outdated Components) |
| `hardening-devsecops-pipelines` | Designs CI/CD gates that block PRs introducing deprecated or EOL dependencies; provides tool configurations for SCA scanners |
| `conducting-threat-modeling` | Includes a dedicated section in the Vulnerability Map for deprecated and CVE-affected dependencies with CVE reference and CVSS score |

**Example prompts:**

```text
# Code review
Scan our backend for deprecated libraries, old or outdated code, and EOL components.
Produce a Real-Time Report with a plan to refactor or replace them.

# Pipeline gate
Help me set up a security gate in GitHub Actions that blocks any new PRs
that introduce libraries or frameworks that are officially deprecated or reaching EOL.

# Vulnerability Map
Produce a Vulnerability Map that includes a table of any deprecated or
CVE-affected dependencies with recommended actions.
```

---

## 25. When should I use threat modeling vs. a code review?

These two skills target different points in the development lifecycle and answer different questions.

| | **Threat modeling** (`devsec-threat-modeling`) | **Code review** (`devsec-code-review`) |
|---|---|---|
| **When to use** | Design phase — before or during implementation | Implementation/review phase — code exists |
| **Input** | Architecture diagrams, system descriptions, data flow descriptions | Actual source code, file paths |
| **Question answered** | "What could go wrong with this design?" | "What is actually wrong in this code?" |
| **Output** | Threat Model Document + Vulnerability Map (threats, risk register, security requirements) | Real-Time Report (findings) + Remediation Guides (fixes) |
| **Framework** | STRIDE per component | OWASP Top 10 + ASVS 5.0 per finding |
| **Best for** | New features, new services, architecture changes | Pull requests, security audits, pre-release reviews |

**Use both together for the highest confidence.** A common effective pattern:

```text
Threat model this API design, then review the existing code for security issues
that match those threats, and finally document the findings as an ADR.
```

This prompt chains `devsec-threat-modeling` → `devsec-code-review` → `doc-engineer` in sequence.

---

## 26. What is "write once, comply many"?

"Write once, comply many" is a principle built into the `managing-compliance-frameworks` skill. It means that a single, well-implemented security control — or a single piece of evidence — can satisfy multiple compliance frameworks simultaneously.

**Example:** A WAF with properly configured rules satisfies:

- **ISO 27001:2022** — Clause 6.1.2 (information security risk treatment)
- **NIST SSDF PW.6** — software protection mechanisms
- **OWASP A03:2021** — injection prevention

Instead of implementing or documenting these separately for each audit, you implement once and map the evidence to all applicable frameworks in a single Compliance Log row.

The agent always surfaces these overlaps when producing compliance outputs. This reduces implementation burden, unifies evidence collection across audits, and avoids the common trap of treating each compliance framework as a separate project.

---

## 27. How do I run an OWASP SAMM maturity assessment?

The `devsec-program` agent performs SAMM assessments. SAMM (Software Assurance Maturity Model) evaluates security maturity across **5 business functions** and **15 security practices**, scoring each on a 0–3 scale.

**The five SAMM business functions:**

| Function | Practices |
|---|---|
| **Governance** | Strategy & Metrics, Policy & Compliance, Education & Guidance |
| **Design** | Threat Assessment, Security Requirements, Security Architecture |
| **Implementation** | Secure Build, Secure Deployment, Defect Management |
| **Verification** | Architecture Assessment, Requirements-driven Testing, Security Testing |
| **Operations** | Incident Management, Environment Management, Operational Management |

**How to request an assessment:**

```text
Run an OWASP SAMM maturity assessment for our engineering org.
Give us a scorecard across Governance, Design, and Implementation.
Context: 200-person startup, no formal AppSec team, we use GitHub Actions
for CI/CD, and we have ad-hoc security reviews before major releases.
```

The more context you provide about your current state — team size, existing tooling, past security incidents, compliance targets — the more accurate the scorecard and the more targeted the improvement roadmap.

The agent produces:

- A SAMM scorecard with current level (0–3) per practice
- A gap analysis identifying the highest-priority improvements
- A phased roadmap (30/60/90 days, 6 months, 12 months) with measurable milestones
- An indication of which SAMM gaps map to compliance requirements (SOC 2, ISO 27001, NIST SSDF)
