# Documentation Writer Agent — AI Core

> **AI Core** | Documentation Engineering | Author every doc type, from any source, in any format.

The **Documentation Writer Agent** is a full-stack documentation engineering AI Core. It converts source code, Jira epics, Confluence pages, OpenAPI specs, and PRDs into accurate, structured documentation — API references, architecture docs, ADRs, release notes, READMEs, tutorials, user guides, Word documents, PDFs, and PowerPoint presentations.

---

## Architecture Overview

```
documentation-writer-agent/
├── agents/
│   ├── atlassian-sourcer.md      ← Retrieves source bundles from Jira & Confluence via MCP
│   ├── c4-architect.md           ← Produces C4 Model architecture diagrams and docs
│   └── doc-engineer.md           ← Primary documentation engineer: all doc types
└── skills/
    ├── authoring-technical-docs/ ← Core quality framework (always load first)
    ├── authoring-api-docs/       ← REST endpoints, SDK references, OpenAPI generation
    ├── authoring-architecture-docs/ ← ADRs, design docs, C4 diagrams
    ├── authoring-release-docs/   ← Release notes, changelogs, READMEs
    ├── authoring-user-docs/      ← Tutorials, how-to guides, user & onboarding guides
    ├── automating-docs-updates/  ← Syncs docs automatically on git commit/push
    ├── editing-docx-files/       ← Create and edit Word documents (.docx)
    ├── editing-pptx-files/       ← Create and edit PowerPoint presentations (.pptx)
    ├── processing-pdfs/          ← Read, merge, split, create, and OCR PDF files
    └── sourcing-from-atlassian/  ← Jira/Confluence MCP retrieval procedures
```

---

## MCP Connections

| Server      | URL                              | Used By                                    |
|-------------|----------------------------------|--------------------------------------------|
| `atlassian` | `https://mcp.atlassian.com/v1/mcp` | `atlassian-sourcer`, `doc-engineer`, `c4-architect` |

The Atlassian MCP connection enables fetching user stories, epics, acceptance criteria, and Confluence pages directly into the documentation workflow. Authentication uses OAuth; the agent calls `getAccessibleAtlassianResources` first on every session to obtain the correct `cloudId`.

---

## Agents

### `doc-engineer`

**Description:** Primary Documentation Engineer. Executes the full research → draft → review → format workflow for any documentation task.

**Triggers:** `"write docs for"`, `"document this"`, `"generate release notes"`, `"create a README"`, `"review these docs"`, `"write a tutorial"`, `"create an ADR"`, `"use the Jira stories"`, `"pull from Confluence"`

**Skills loaded:** `authoring-technical-docs`, `authoring-api-docs`, `authoring-architecture-docs`, `authoring-release-docs`, `authoring-user-docs`, `editing-docx-files`, `processing-pdfs`, `editing-pptx-files`, `automating-docs-updates`, `sourcing-from-atlassian`

**Examples:**
```
"Document the /payments REST endpoint from the OpenAPI spec"
"Write release notes for v2.3.0 from these Jira tickets"
"Create a README that lets a new developer clone and run the project in under 30 seconds"
"Review the existing architecture docs and give me a quality verdict"
"Write a tutorial for our new auth SDK — audience is external developers"
"Generate a Word document version of this API reference"
```

---

### `atlassian-sourcer`

**Description:** Precision research agent that fetches Jira user stories, epics, acceptance criteria, sprint tickets, and Confluence pages via MCP, then returns a structured **source bundle** for downstream agents.

**Triggers:** `"pull user stories from Jira"`, `"get the Confluence spec"`, `"fetch the epic"`, `"source from Atlassian"`, `"read the JIRA ticket"`, `"include the acceptance criteria"`

**Skills loaded:** `sourcing-from-atlassian`

**Examples:**
```
"Pull the epic PROJ-42 and all its child stories"
"Fetch the Confluence page 'Auth Service Design' from the ENG space"
"Get all sprint tickets for the checkout feature"
"Search for everything related to the payments module across Jira and Confluence"
```

---

### `c4-architect`

**Description:** Software Architect specializing in the C4 Model. Produces context (C1), container (C2), component (C3), and deployment diagrams scoped to the right SDLC phase. Integrates with Atlassian to populate diagrams from epics and design pages.

**Triggers:** `"document the architecture"`, `"generate a C4 context diagram"`, `"show me the container architecture"`, `"create an ADR"`, `"visualize this system"`

**Skills loaded:** `authoring-technical-docs`, `authoring-architecture-docs`, `sourcing-from-atlassian`

**Examples:**
```
"Generate a C4 context diagram for our e-commerce platform"
"Show me the container architecture for the auth service"
"Document the components in the payment module"
"Create an ADR for switching from REST to GraphQL"
"Map the Jira epic PROJ-100 to a C2 container diagram"
```

---

## Skills

### `authoring-technical-docs` ⭐ *(load first)*

**Description:** Core documentation engineering action. Provides the quality framework, style rules, and multi-pass workflow (research → draft → review → format) that all documentation must follow.

**When to use:** Always. This is the foundation every other skill builds on. If no domain skill matches, this skill alone is sufficient.

**Key features:**
- Diátaxis framework classification (Tutorial / How-To / Explanation / Reference)
- Six quality dimensions: Accuracy, Completeness, Usability, Consistency, Readability, Structure
- Gap detection with blocker/warning/info severity
- YAML frontmatter requirements for every document

**Example:**
```
"Review this existing API reference doc — give me a quality verdict"
"Write documentation for this Python module (general, no specific type needed)"
```

---

### `authoring-api-docs`

**Description:** Produces REST endpoint documentation, SDK/library references, CLI command references, and documentation generated from OpenAPI/Swagger specs.

**Triggers:** `"document this API"`, `"generate API reference"`, `"write SDK docs"`, `"document these endpoints"`

**Key features:**
- REST endpoint template (auth, parameters, request/response, error codes, rate limits)
- SDK function template (params, return values, errors, usage example)
- OpenAPI/Swagger → full reference generation
- Every endpoint gets a copy-pasteable working example

**Example:**
```
"Generate API reference documentation from this OpenAPI spec"
"Document the /users/{id} endpoint — include auth, params, errors, and a working curl example"
"Write SDK docs for the Python client library"
```

---

### `authoring-architecture-docs`

**Description:** Produces Architecture Decision Records (ADRs), design documents, system architecture overviews, and C4 Model diagrams scoped to the current SDLC phase.

**Triggers:** `"write a design doc"`, `"create an ADR"`, `"document the architecture"`, `"write a technical proposal"`

**Key features:**
- C4 Model at all four levels (Context, Container, Component, Deployment + Dynamic)
- SDLC phase → diagram level selection matrix
- Mermaid diagram generation (version-controllable)
- ADR and design document templates
- Anti-pattern detection

**Example:**
```
"Create an ADR for our decision to use PostgreSQL over MongoDB"
"Write a system architecture overview for our microservices platform"
"Generate C4 context and container diagrams for the order processing system"
"Write a technical design document for the new notification service"
```

---

### `authoring-release-docs`

**Description:** Produces release notes, changelogs, READMEs, and migration guides from Jira exports, commit logs, and PR summaries.

**Triggers:** `"write release notes"`, `"generate a changelog"`, `"create a README"`, `"document what changed"`, `"write migration guide"`

**Key features:**
- User-impact-first framing (not implementation detail)
- Structured section order: Breaking Changes → Features → Improvements → Bug Fixes → Deprecations
- Breaking changes in prominent callout blocks with inline migration steps
- Every bug fix and feature entry references its issue tracker ID
- README standalone standard (clone-to-run in under 30 seconds)

**Example:**
```
"Write release notes for v3.1.0 from these Jira tickets"
"Generate a CHANGELOG.md from this git log"
"Create a migration guide from API v1 to v2"
"Write a README that lets a new contributor get started in under 30 seconds"
```

---

### `authoring-user-docs`

**Description:** Produces tutorials, how-to guides, user guides, getting-started guides, and onboarding documentation for end-users and developers learning to use software.

**Triggers:** `"write a tutorial"`, `"create a getting started guide"`, `"document how to use this"`, `"write a user guide"`, `"create onboarding docs"`

**Key features:**
- Diátaxis-aligned type selection (Tutorial / How-To / User Guide / Onboarding Guide)
- "Hello World" rule — give users a small win early
- Idempotent steps (safe to re-run)
- Validation step at the end of every tutorial
- Role-based onboarding (developer, QA, DevOps, PM)

**Example:**
```
"Write a tutorial for first-time users of our CLI tool"
"Create a how-to guide for configuring SSO with our app"
"Write an onboarding guide for new backend engineers joining the team"
"Create a comprehensive user guide for the dashboard feature"
```

---

### `automating-docs-updates`

**Description:** Automatically intercepts git commit/push workflows, analyzes the staged changes, and updates the relevant documentation before the commit is finalized.

**Triggers:** `"commit and push"` (when documentation should be kept in sync)

**Key features:**
- Reads `git diff --cached` to identify what changed
- Determines which docs cover the modified code
- Updates API docs, architecture overviews, or user guides as appropriate
- Stages updated doc files alongside the code changes

**Example:**
```
"Commit these changes to the payment module and keep the docs in sync"
"Push this PR — make sure the API reference reflects the new endpoint parameters"
```

---

### `editing-docx-files`

**Description:** Create, read, edit, and manipulate Word documents (.docx) — including tables of contents, headers, footers, tracked changes, comments, and images.

**Triggers:** `"Word doc"`, `"word document"`, `.docx`, `"report"`, `"memo"`, `"letter"`, professional formatted deliverable

**Key features:**
- Create from scratch using `docx-js` (JavaScript library)
- Edit existing documents via XML unpack → edit → repack workflow
- Full tracked changes and comments support
- Table/image/TOC generation
- Validation and auto-repair

**Example:**
```
"Convert this Markdown API reference to a professionally formatted Word document"
"Add tracked-change suggestions to this .docx contract"
"Create a Word report with a table of contents, headers, and page numbers"
"Extract the text from this .docx file"
```

---

### `editing-pptx-files`

**Description:** Create, read, edit, and manipulate PowerPoint presentations (.pptx) — including slide decks, pitch decks, speaker notes, and design-polished presentations.

**Triggers:** `"deck"`, `"slides"`, `"presentation"`, `.pptx`, any mention of PowerPoint

**Key features:**
- Read/extract text with `markitdown`
- Create from scratch with `pptxgenjs`
- Edit existing decks via XML unpack → edit → repack workflow
- Curated design guidance (color palettes, typography, layout options)
- Visual QA via image conversion

**Example:**
```
"Create a 10-slide pitch deck for our product launch"
"Extract the text from this PowerPoint and summarize it"
"Update slide 3 in this presentation with the new Q3 metrics"
"Create a technical presentation for our API documentation"
```

---

### `processing-pdfs`

**Description:** Read, extract text/tables from, merge, split, rotate, watermark, create, fill forms on, encrypt/decrypt, and OCR PDF files.

**Triggers:** any `.pdf` file, `"extract text from PDF"`, `"merge PDFs"`, `"create a PDF"`, `"fill this form"`

**Key features:**
- Text extraction with `pypdf` and `pdfplumber`
- Table extraction to Pandas DataFrames / Excel
- PDF creation with `reportlab`
- OCR for scanned PDFs via `pytesseract`
- PDF form filling (see FORMS.md)
- Merge, split, rotate, watermark, encrypt/decrypt

**Example:**
```
"Extract all tables from this PDF report and convert them to Excel"
"Merge these three PDFs into one"
"Generate a PDF version of this release notes document"
"OCR this scanned contract and extract the text"
```

---

### `sourcing-from-atlassian`

**Description:** MCP retrieval procedures for fetching Jira issues, epics, user stories, acceptance criteria, sprint tickets, and Confluence pages. Used by `atlassian-sourcer` and optionally by `doc-engineer` and `c4-architect`.

**Triggers:** (Used internally by other agents; can also be loaded directly when Atlassian data is needed)

**Key features:**
- Complete JQL query patterns for epics, sprints, releases, features
- CQL query patterns for Confluence page discovery
- Field extraction maps (Jira → documentation concepts)
- Staleness and contradiction detection
- Source bundle format (structured output for downstream consumption)
- Error handling for MCP failures

**Example:**
```
"Use the Jira epic PROJ-42 and all its stories as the documentation source"
"Fetch the Confluence pages tagged 'api-design' in the ENG space"
"Pull all done stories from the v2.1.0 fix version for the release notes"
```

---

## Installation

### Install the full AI Core (all agents)
```bash
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent -a gemini
```

### Install a single agent
```bash
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/agents/doc-engineer -a gemini
```

### Install a single skill
```bash
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/documentation-writer-agent/skills/authoring-technical-docs -a gemini
```
