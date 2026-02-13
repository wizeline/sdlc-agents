# Architecture Documentation Reference

This reference covers architecture decision records (ADRs), design documents, and system architecture overviews — the understanding-oriented quadrant of the Diátaxis framework.

## Template: Architecture Decision Record (ADR)

ADRs capture the "why" behind technical decisions. They're one of the most valuable forms of documentation because they preserve context that is otherwise lost.

```markdown
---
title: "ADR-[number]: [Short decision title]"
status: [proposed | accepted | deprecated | superseded by ADR-XXX]
date: [YYYY-MM-DD]
decision-makers: [List of people involved]
---

# ADR-[number]: [Short decision title]

## Status

[Proposed | Accepted | Deprecated | Superseded by [ADR-XXX](link)]

## Context

[Describe the situation that requires a decision. What problem are you facing? What constraints exist? What forces are at play? Be specific — include numbers, timelines, and technical constraints.]

## Decision

[State the decision clearly in one sentence, then elaborate.]

We will [decision].

[Explain the decision in more detail. What does this mean concretely?]

## Consequences

### Positive

- [Benefit 1]
- [Benefit 2]

### Negative

- [Trade-off 1]
- [Trade-off 2]

### Neutral

- [Side effect that is neither clearly good nor bad]

## Alternatives considered

### [Alternative 1]

[What it was and why it was rejected.]

### [Alternative 2]

[What it was and why it was rejected.]

## References

- [Link to related ADRs, design docs, or external resources]
```

## Template: Design Document

For longer-form technical design proposals:

```markdown
---
title: "[Feature/System] design document"
author: [Name]
status: [draft | under review | approved | implemented]
date: [YYYY-MM-DD]
reviewers: [List]
---

# [Feature/System] design document

## Summary

[One paragraph: what this document proposes and why. A busy engineer should be able to decide whether to keep reading based on this paragraph alone.]

## Goals and non-goals

### Goals

- [What this design aims to achieve]
- [Measurable success criteria if possible]

### Non-goals

- [What this design explicitly does NOT address]
- [Scope boundaries]

## Background

[Context needed to understand the proposal. Current state of the system, relevant history, user needs. Keep it factual.]

## Detailed design

### Overview

[High-level description of the approach. Include a system diagram if helpful.]

### [Component / aspect 1]

[Detailed description. Include data models, API contracts, algorithms, or whatever is relevant.]

### [Component / aspect 2]

[Continue for each major aspect.]

### Data model

[If applicable: schema definitions, entity relationships, migration plan.]

### API changes

[If applicable: new or modified endpoints, request/response formats.]

## Security considerations

[Authentication, authorization, data privacy, encryption, threat model.]

## Performance considerations

[Expected load, latency requirements, scaling strategy, resource usage.]

## Testing strategy

[How this will be tested: unit tests, integration tests, load tests, rollback plan.]

## Migration / rollout plan

[How to get from current state to proposed state. Feature flags, phased rollout, backward compatibility.]

## Open questions

- [Question that still needs answering before implementation]
- [Another open question]

## Appendix

[Supporting data, benchmarks, reference implementations, links.]
```

## Template: System Architecture Overview

For documenting the current state of a system:

```markdown
---
title: "[System name] architecture overview"
audience: [new team members / architects / external integrators]
last-updated: [YYYY-MM-DD]
---

# [System name] architecture overview

## What [system name] does

[2-3 sentences: the system's purpose in business terms, not technical terms.]

## Architecture diagram

[Include or reference a diagram. Describe what it shows.]

## Components

### [Component 1]

- **Purpose:** [What it does]
- **Technology:** [Language, framework, infrastructure]
- **Owns:** [What data or functionality this component is responsible for]
- **Communicates with:** [Other components it talks to and how]

### [Component 2]

[Same structure.]

## Data flow

[Describe how data moves through the system. Use a sequence diagram or numbered steps.]

1. [User/system initiates action]
2. [Component A receives and processes]
3. [Component A sends to Component B via protocol]
4. [Component B persists / returns / forwards]

## Infrastructure

| Resource | Provider | Purpose |
|----------|----------|---------|
| [Database] | [AWS RDS / self-hosted] | [Primary data store] |
| [Queue] | [SQS / Kafka] | [Async message processing] |

## Authentication and authorization

[How users and services authenticate. Token flow, service-to-service auth.]

## Monitoring and observability

[What's monitored, where dashboards live, alerting strategy.]

## Key technical decisions

[Link to relevant ADRs. Briefly summarize the decisions that shaped this architecture.]

## Known limitations

[Current constraints, tech debt, things that don't scale or are fragile.]

## Glossary

[Domain-specific terms used in this document.]
```

## Rules Specific to Architecture Docs

1. **Separate "what is" from "what should be."** Architecture overviews describe the current state. Design docs describe the proposed future state. Never mix them.
2. **Diagrams are not optional.** Architecture docs without diagrams are architecture opinions. Use Mermaid, PlantUML, or describe the diagram for the user to create.
3. **Include the "why."** The most common complaint about architecture docs is that they explain *what* the system does but not *why* it was built that way. ADRs solve this.
4. **Write for the new team member.** The primary audience for architecture docs is someone joining the team who needs to build a mental model of the system.
5. **Keep it updated or mark it stale.** An outdated architecture doc is worse than no doc. Include a `last-updated` date and flag docs that haven't been updated in 6+ months.
6. **Be honest about trade-offs.** Every architecture has weaknesses. Document them. This builds trust and prevents surprises.
7. **Link to code.** When describing a component, link to the relevant repo, directory, or entry point file.
