# Documentation Writer — Prompt Examples

Ready-to-use prompts for the **Documentation Writer** AI Core.
This core includes **2 agents** (`doc-engineer`, `c4-architect`) backed by **8 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically load the required skills, research the codebase, and produce the documentation.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
|---|---|
| `doc-engineer` | `authoring-technical-docs`, `authoring-api-docs`, `authoring-architecture-docs`, `authoring-release-docs`, `authoring-user-docs`, `editing-docx-files`, `processing-pdfs`, `editing-pptx-files` |
| `c4-architect` | `authoring-architecture-docs` |

---

## `doc-engineer` — General Documentation

### API Documentation (`authoring-api-docs`)

```text
Write docs for the /api/v2/orders endpoint. Include request/response schemas,
auth requirements, error codes, and a cURL example.
```

```text
Document this OpenAPI spec as a developer-friendly API reference with
authentication guides and rate-limit policies.
```

```text
Generate an SDK reference for our Python client library. Cover installation,
authentication, each method signature, and a complete working example per method.
```

### Architecture Documentation (`authoring-architecture-docs`)

```text
Write a design doc / ADR for migrating our monolith to microservices. Cover the
decision drivers, trade-offs, and phased rollout plan.
```

```text
Create an architecture overview document for our event-driven data pipeline
including system context and container-level views.
```

```text
Document our authentication architecture: the OAuth 2.0 flow, token lifecycle,
refresh strategy, and how services validate tokens at each trust boundary.
```

### Release Documentation (`authoring-release-docs`)

```text
Generate release notes for v3.2.0 from the git log between tags v3.1.0 and
v3.2.0. Group by features, fixes, and breaking changes.
```

```text
Create a README for this project. It should include quick start instructions,
prerequisites, configuration, and contribution guidelines.
```

```text
Write a migration guide from v2 to v3. List every breaking change, provide
before/after code snippets, and include a step-by-step upgrade checklist.
```

### User Documentation (`authoring-user-docs`)

```text
Write a tutorial for setting up local development, from cloning the repo to
running the first test suite successfully.
```

```text
Create a how-to guide for configuring SSO with Azure AD in our application,
with step-by-step instructions and screenshots.
```

```text
Write a getting-started guide for new team members. Cover repo setup, dev
environment, running the app locally, and making their first contribution.
```

### Documentation Review (`authoring-technical-docs`)

```text
Review these docs in docs/architecture/ for completeness, accuracy, and style.
Classify issues by severity (Blocker / Major / Minor).
```

```text
Audit our entire docs/ folder. Identify outdated content, broken links,
missing topics, and inconsistencies with the current codebase.
```

### Format Conversion (`editing-docx-files` / `processing-pdfs` / `editing-pptx-files`)

```text
Convert the architecture document at docs/system-design.md into a Word document
using the company brand template.
```

```text
Export the API reference to PDF with syntax-highlighted code blocks and a
table of contents.
```

```text
Create a PowerPoint presentation summarizing our system architecture for the
stakeholder meeting.
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

1. API Reference (authoring-api-docs)
   Document every endpoint under /api/v1/orders and /api/v1/customers.
   Include request/response schemas with examples, authentication
   requirements, error codes, rate-limit policies, and runnable cURL
   examples for each operation.

2. Architecture Overview & C4 Diagrams (authoring-architecture-docs + c4-architect)
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
release. Produce: (1) full API reference for /api/v1/orders and /customers
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
package. Please: (1) document how the system works for developers who will
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
