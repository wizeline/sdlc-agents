---
name: authoring-architecture-docs
description: "Use when producing architecture and design documentation — Architecture Decision Records (ADRs), design documents, system architecture overviews, or technical design proposals. Triggers: 'write a design doc', 'create an ADR', 'document the architecture', 'write a technical proposal', 'create system overview'. Always load authoring-technical-docs first."
---
# Authoring Architecture Docs Action

Produces architecture decision records, design documents, and C4 Model diagrams scoped to the appropriate level for the current SDLC phase and audience — the understanding-oriented quadrant of the Diátaxis framework. Diagrams are scoped to the appropriate level for the current SDLC phase and audience.

**Load `authoring-technical-docs` first** for the multi-pass workflow, style rules, and quality framework. This action provides the templates and architecture-specific rules.

---

## References

Read these before executing any step.

| File                            | Contains                                                |
| ------------------------------- | ------------------------------------------------------- |
| `references/notation.md`      | C4 abstractions, Mermaid element types, verb bank       |
| `references/level-rules.md`   | Per-level rules and forbidden elements                  |
| `references/sdlc-mapping.md`  | SDLC phase → diagram selection, audience decision tree |
| `references/anti-patterns.md` | Mistakes to detect and avoid                            |

## Assets

Use these as output scaffolding.

| File                                                      | Use for                                            |
| --------------------------------------------------------- | -------------------------------------------------- |
| `assets/architecture-doc.md`                            | Top-level ARCHITECTURE.md structure                |
| `assets/diagram-templates.md`                           | Per-level section templates (fill in placeholders) |
| `assets/Architecture_Decision_Record_(ADR)_template.md` | Template for Architecture Decision Records (ADRs)  |
| `assets/Design_document_template.md`                    | Template for technical design documents            |
| `assets/System_architecture_template.md`                | Template for system architecture overviews         |

---

## Workflow

### Step 1 — Read references

Read all four files in `references/` before any other action.

### Step 2 — Gather project context

Extract from input. Ask only if a required field is missing.

| Field                                    | Required for             |
| ---------------------------------------- | ------------------------ |
| System name and purpose                  | All levels               |
| Users / personas                         | L1+                      |
| External integrations                    | L1+                      |
| Tech stack                               | L2+                      |
| SDLC phase                               | Selecting diagram levels |
| Target container name + source structure | L3 only                  |
| Flow name and step-by-step description   | Dynamic only             |

### Step 3 — Select diagram levels

Consult `references/sdlc-mapping.md` → Phase Matrix and Audience Decision Tree.
State the selection and rationale before proceeding.

### Step 4 — Produce each diagram

For each selected level, in order:

1. Open `assets/diagram-templates.md` and copy the matching template section
2. Fill every `{{placeholder}}` — leave none blank
3. Apply all rules from `references/level-rules.md` for that level
4. Scan output against `references/anti-patterns.md`
5. Self-check against the Review Checklist in `assets/architecture-doc.md`

### Step 5 — Assemble output document

Open `assets/architecture-doc.md` as the document scaffold.
Insert completed diagram sections in level order: L1 → L2 → L3 → Deployment → Dynamic.
Complete the Key Architectural Decisions table and Change Log.

### Step 6 — Save files

```
docs/architecture/ARCHITECTURE.md
docs/architecture/diagrams/context.mermaid
docs/architecture/diagrams/container.mermaid
docs/architecture/diagrams/component-{{container_name}}.mermaid
docs/architecture/diagrams/deployment.mermaid
docs/architecture/diagrams/dynamic-{{flow_name}}.mermaid
```

Save only the files corresponding to the levels actually produced.

---

## Inputs

- Project description, README, or source code
- SDLC phase (required)
- Target container name and source structure (L3 only)
- Flow name and steps (Dynamic only)

## Outputs

- `ARCHITECTURE.md` — full assembled documentation package
- `diagrams/*.mermaid` — one file per diagram level produced

---

## Architecture docs rules

1. **Separate "what is" from "what should be."** Overviews describe current state. Design docs describe proposed future.
2. **Diagrams are not optional.** Use Mermaid — it's version-controllable.
3. **Include the "why."** Link to ADRs.
4. **Write for the new team member.**
5. **Be honest about trade-offs.** Every architecture has weaknesses. Document them.
6. **Link to code.** When describing a component, link to the relevant repo or entry point.

---

## Using Mermaid for diagrams

Accompany each diagram with a brief prose summary (2–4 sentences) explaining the key relationships shown.

**System overview:**

```mermaid
graph TB
    User[User] --> WebApp[Web Application]
    WebApp --> API[API Service]
    API --> DB[(Database)]
    API --> Queue[Message Queue]
    Queue --> Worker[Background Worker]
    Worker --> DB
```

**Sequence diagram:**

```mermaid
sequenceDiagram
    participant Client
    participant API
    participant DB
    Client->>API: POST /resources
    API->>DB: INSERT resource
    DB-->>API: resource_id
    API-->>Client: 201 Created
```

## Example Triggers

- "Generate a C4 context diagram for this project"
- "Show me the container architecture"
- "Document the components in the auth service"
- "Create a class diagram for the payment module"