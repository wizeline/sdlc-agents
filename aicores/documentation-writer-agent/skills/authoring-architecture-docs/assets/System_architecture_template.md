---
title: "[System name] architecture overview"
doc-type: explanation
audience: [new team members | architects | external integrators]
last-updated: [YYYY-MM-DD]
version: [X.X]
---
# [System name] architecture overview

## What [system name] does

[2-3 sentences: the system's purpose in business terms, not technical terms.]

## Architecture diagram

[Mermaid diagram]

## Components

### [Component 1]

- **Purpose:** [What it does]
- **Technology:** [Language, framework, infrastructure]
- **Owns:** [What data or functionality it's responsible for]
- **Communicates with:** [Other components and how]

## Data flow

1. [User/system initiates action]
2. [Component A receives and processes]
3. [Component A sends to Component B via protocol]

## Infrastructure

| Resource   | Provider  | Purpose              |
| ---------- | --------- | -------------------- |
| [Database] | [AWS RDS] | [Primary data store] |

## Key technical decisions

[Link to relevant ADRs.]

## Known limitations

[Current constraints, tech debt, fragile areas.]
