# Documentation Writer — Prompt Examples

Ready-to-use prompts for the **Documentation Writer** AI Core.
This core includes **3 agents** (`doc-engineer`, `c4-architect`, `atlassian-sourcer`) backed by **11 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically load the required skills, research the codebase, and produce the documentation.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
| --- | --- |
| `doc-engineer` | `authoring-technical-docs`, `authoring-api-docs`, `authoring-architecture-docs`, `authoring-release-docs`, `authoring-user-docs`, `editing-docx-files`, `processing-pdfs`, `editing-pptx-files`, `automating-docs-updates`, `sourcing-from-atlassian` |
| `c4-architect` | `authoring-architecture-docs`, `sourcing-from-atlassian` |
| `atlassian-sourcer` | `sourcing-from-atlassian` |

---

## `doc-engineer` — Technical Documentation

The `authoring-technical-docs` skill is the **core foundation** for all tasks. Use it alone for general documentation needs, or combine it with domain extensions below.

### Core Documentation (`authoring-technical-docs`)

*Use for general writing, specs, and refining existing documentation.*

```text
Write a technical spec for our new rate-limiting system based on these architecture notes.
```

```text
Review this draft doc and improve it to meet professional engineering standards. Fix tone, clarity, and technical accuracy.
```

```text
I have these Jira tickets and engineering notes — produce a polished technical document from them.
```

```text
Audit our entire docs/ folder. Identify outdated content, broken links, missing topics, and inconsistencies with the current codebase.
```

### Atlassian Sourcing (`sourcing-from-atlassian`)

*Fetch user stories, epics, sprint tickets, and Confluence pages before drafting.*

```text
Use the Jira stories for Epic-123 and the Confluence spec in the ENG space to create a documentation source bundle.
```

```text
Pull the user stories for the new login flow from Jira and the corresponding design doc from Confluence.
```

### API Documentation (`authoring-api-docs`)

*REST endpoints, SDK references, CLI docs, OpenAPI/Swagger specs.*

```text
Document this Express.js router — generate a full REST API reference for each endpoint, including request/response schemas, error codes, and a cURL example.
```

```text
I have an OpenAPI spec. Turn it into a human-readable API reference doc with authentication guides and rate-limit policies.
```

```text
Write SDK reference docs for this Python client library's public functions. Cover installation, methods, and a working example per method.
```

```text
Generate API docs for this POST /users endpoint, including all error codes and a curl example.
```

### Architecture Documentation (`authoring-architecture-docs`)

*ADRs, design docs, system overviews, C4 diagrams, technical proposals.*

```text
Write an ADR for choosing PostgreSQL over MongoDB for our new service. Cover decision drivers, trade-offs, and the final recommendation.
```

```text
Create a C4 context diagram for our microservices payment system using Mermaid, showing external actors and system boundaries.
```

```text
Write a design doc for our proposed event-driven notification system. Include the system overview, data flow, and failure handling strategy.
```

```text
Document this codebase's architecture as a system overview for new engineers. Cover the main components, their interactions, and the tech stack.
```

### Release Documentation (`authoring-release-docs`)

*Release notes, changelogs, READMEs, migration guides.*

```text
Here are 30 merged PRs — write user-facing release notes for version 2.4.0. Group by features, fixes, and breaking changes.
```

```text
Generate a changelog from this list of Jira tickets and git logs.
```

```text
Write a migration guide for users upgrading from v1 to v2 — list every breaking change, provide before/after code snippets, and include an upgrade checklist.
```

```text
Create a comprehensive README for this open-source CLI tool including setup, configuration, and contribution guides.
```

### User Documentation (`authoring-user-docs`)

*Tutorials, how-to guides, onboarding, getting-started.*

```text
Write a beginner tutorial for setting up our SDK — assume zero prior experience and cover everything from cloning to the first "hello world".
```

```text
Create a how-to guide for connecting our app to a Slack workspace, explaining the configuration steps and bot permissions.
```

```text
Write an onboarding guide for new developers joining this project. Cover environment setup, running the app locally, and making their first PR.
```

```text
Document how to use the dashboard's filter feature as a user guide chapter for non-technical users.
```

### Docs Updates on Commit (`automating-docs-updates`)

*Automatically handle documentation updates whenever committing changes.*

```text
I am about to commit these changes to the `user-service`. Please update the affected API documentation and architecture overviews, and stage the docs so they can be committed together.
```

### Format Skills (`editing-docx-files` / `processing-pdfs` / `editing-pptx-files`)

*Create, edit, or manipulate specific file formats.*

```text
Create a professional memo in Word format (.docx) summarizing these meeting notes. Update the table of contents and fix the formatting.
```

```text
Convert this markdown file into a polished Word document with professional headings, page numbers, and our company brand styles.
```

```text
Create a 10-slide pitch deck (.pptx) based on these bullet points. Include slides for problem, solution, architecture, and roadmap.
```

```text
Extract and summarize all text from this uploaded presentation, then update slide 5 to reflect our new Q4 sales figures.
```

```text
Merge these three PDF reports into one document, extract all text from the scanned invoice page, and add a "CONFIDENTIAL" watermark to every page.
```

```text
Fill in this PDF form with the data from this JSON file and export the final result.
```

---

## `c4-architect` — C4 Model Architecture

```text
Document our payment processing system's architecture using the C4 Model.
We're in the design phase, so focus on Context and Container views.
```

```text
Map the architecture of our Kubernetes-deployed microservices system using C4.
Show the Context, Container, and Component diagrams for the order service.
```

```text
Visualize our system's integration with third-party APIs (Stripe, SendGrid,
Auth0) using C4 Context and Container diagrams.
```

```text
We just finished the detailed design. Create a C4 Component diagram for the
notification service showing internal classes, their responsibilities, and
how they interact with the message queue and database.
```

---

## Full Suite — All Agents & Skills in One Prompt

The following prompt is designed to exercise **every agent and skill** in the Documentation Writer AI Core in a single call. Use it as a starting point and adapt the project name, tech stack, and audience to your own context.

```text
We are launching the "OrderFlow" platform — a microservices-based order
management system built with Node.js (Express), deployed on AWS EKS, with a
PostgreSQL database and a Redis cache layer. The API is RESTful and secured
via OAuth 2.0 with PKCE. We need a complete documentation package for the
v1.0.0 release. Please produce every deliverable below:

0. Source Gathering (sourcing-from-atlassian)
   Fetch the epic "ORDER-100" and the "OrderFlow Spec" page from the ENG
   Confluence space to build an Atlassian source bundle. Use this bundle
   as context for all following deliverables.

1. API Reference (authoring-api-docs)
   Document every endpoint under /api/v1/orders and /api/v1/customers.
   Include request/response schemas with examples, authentication
   requirements, error codes, rate-limit policies, and runnable cURL
   examples for each operation.

2. Architecture Overview & C4 Diagrams (authoring-architecture-docs + c4-architect + atlassian-sourcer)
   Write an architecture overview document with:
   - A C4 Context diagram showing OrderFlow, its external actors (web app,
     mobile app, payment gateway, shipping provider), and system boundaries.
   - A C4 Container diagram showing the API gateway, order service, customer
     service, notification service, PostgreSQL, and Redis.
   - A C4 Component diagram for the order service showing internal modules,
     their responsibilities, and interactions.
   - An ADR documenting the decision to use event-driven communication
     (via Amazon SNS/SQS) between services instead of synchronous REST calls.

3. Release Documentation (authoring-release-docs)
   - Generate release notes for v1.0.0 grouped by features, known
     limitations, and deployment prerequisites.
   - Create a comprehensive README with quick start, prerequisites,
     environment variables, configuration, and contribution guidelines.
   - Write a migration guide for teams moving from our legacy monolith to
     OrderFlow, with before/after code snippets and a step-by-step checklist.

4. User Documentation (authoring-user-docs)
   - Write a getting-started tutorial for new developers: clone, configure,
     run locally, execute the test suite, and make their first API call.
   - Create a how-to guide for configuring SSO with Azure AD for the
     OrderFlow admin dashboard, with step-by-step instructions.

5. Documentation Quality Review (authoring-technical-docs)
   After producing all documents above, run a self-review applying the six
   quality dimensions. Classify any issues found as Blocker / Major / Minor
   and fix blockers before delivering.

6. Format Conversion (editing-docx-files + processing-pdfs + editing-pptx-files)
   - Export the architecture overview as a Word document (.docx).
   - Export the API reference as a PDF with syntax-highlighted code blocks
     and a table of contents.
   - Create a PowerPoint presentation (.pptx) summarizing the system
     architecture, key decisions, and release highlights for the executive
     stakeholder meeting.

Target audience: engineering team (technical docs) and VP of Engineering
(executive presentation). Use active voice, second person, present tense.
```

### What This Prompt Activates

| Step | Agent | Skill(s) |
| ---- | ----- | -------- |
| 0. Source Gathering | `doc-engineer` → `atlassian-sourcer` | `sourcing-from-atlassian` |
| 1. API Reference | `doc-engineer` | `authoring-technical-docs` → `authoring-api-docs` |
| 2. Architecture & C4 | `doc-engineer` + `c4-architect` | `authoring-technical-docs` → `authoring-architecture-docs` |
| 3. Release Docs | `doc-engineer` | `authoring-technical-docs` → `authoring-release-docs` |
| 4. User Docs | `doc-engineer` | `authoring-technical-docs` → `authoring-user-docs` |
| 5. Quality Review | `doc-engineer` | `authoring-technical-docs` (review procedure) |
| 6a. Word Export | `doc-engineer` | `editing-docx-files` |
| 6b. PDF Export | `doc-engineer` | `processing-pdfs` |
| 6c. PowerPoint | `doc-engineer` | `editing-pptx-files` |

> **💡 Tip**: This is a large prompt. Depending on your AI assistant's context window and tool access, it may process the steps sequentially or ask you to confirm before proceeding to the next deliverable. You can also split it into numbered sub-tasks if you prefer more control.

### Short Version — Same Coverage, Fewer Words

If you prefer a concise prompt that still triggers every agent and skill:

```text
Document our Node.js/Express microservices API on AWS EKS for the v1.0.0
release. Produce: (0) fetch the Jira epic ORDER-100 and Confluence spec for context; (1) full API reference for /api/v1/orders and /customers
with schemas, auth, errors, and cURL examples; (2) architecture overview with
C4 Context, Container, and Component diagrams plus an ADR for our event-driven
design; (3) release notes, README, and monolith-to-microservices migration
guide; (4) getting-started tutorial and SSO configuration how-to; (5) self-
review all docs for quality and fix any blockers; (6) export architecture to
DOCX, API reference to PDF, and a stakeholder summary to PPTX.
```

### For Non-Technical Users

A plain-language prompt that still triggers every agent and skill:

```text
We're about to launch a new product and I need a complete documentation
package. Please: (0) pull the relevant Jira tickets and Confluence pages for context; (1) document how the system works for developers who will
use our API, with examples they can copy and paste; (2) create diagrams that
show how the different parts of the system connect, and write up why we
chose the current design; (3) write the release announcement, a project
README, and a guide for teams migrating from the old system; (4) write a
beginner-friendly tutorial for new team members and a step-by-step guide for
setting up single sign-on; (5) review everything you wrote for quality and
fix any issues; (6) export the architecture diagrams to Word, the API docs
to PDF, and create a PowerPoint summary I can present to leadership.
```

---

## Tips for Better Results

1. **Be specific about scope** — Name the service, module, or endpoint you want documented.
2. **State your audience** — "for new developers", "for stakeholders", "for auditors" changes tone and depth.
3. **Mention the output format** — Default is Markdown. Ask for DOCX, PDF, or PPTX if needed.
4. **Provide context** — Mention the tech stack, deployment target, or compliance requirements so the agent can tailor content.
5. **Iterate** — Ask the agent to review and refine its own output for quality.
