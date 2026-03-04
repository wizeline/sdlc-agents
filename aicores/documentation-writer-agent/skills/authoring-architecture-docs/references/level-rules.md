# C4 Level Rules & Constraints

Apply these rules strictly. Level purity is the most common C4 failure mode.

---

## L1 — System Context

**Allowed elements**: `Person`, `System`, `System_Ext`
**Forbidden**: containers, components, databases, deployment nodes
**Relationships**: verb label required; protocol optional (only if known)
**Size limit**: ~12 nodes max — larger systems need a System Landscape first
**Audience**: Everyone — stakeholders, PMs, developers, executives

---

## L2 — Container

**Allowed elements**: `Container`, `ContainerDb`, `ContainerQueue` inside `System_Boundary`
**Forbidden**: internal components, infrastructure nodes (k8s, VMs)
**Relationships**: verb label + protocol/technology both required
**Show Persons**: only at the entry point (Web App, API Gateway, Mobile App)
**External systems**: shown outside the boundary; indicate which container talks to each
**Size limit**: ~20 nodes — split by bounded context if larger
**Audience**: Developers, Architects, DevOps

---

## L3 — Component

**Scope**: ONE container only — never reveal internals of other containers
**Allowed elements**: `Component` inside `Container_Boundary`
**External dependencies**: other containers (DB, queues, services) sit outside the boundary
**Granularity**: group related files/classes into logical components — not 1-file = 1-component
**Forbidden**: method signatures, field names, SQL — those belong to L4
**Size limit**: 5–15 components — more than 20 suggests the container should be split
**Audience**: Developers working on that container

---

## L4 — Code

**Scope**: ONE component only
**Diagram types**: `classDiagram` (OOP) or `erDiagram` (data model)
**Show**: public interface, significant relationships, cardinality
**Omit**: trivial getters/setters, constructors without logic
**Optional**: only produce when complexity justifies it
**Audience**: Developers implementing or reviewing the component

---

## Deployment

**Elements**: `Deployment_Node` wrapping `Container`/`ContainerDb` instances
**Show**: cloud provider, region, compute layer, managed services, networking
**Forbidden**: internal components — stay at container level
**Ports**: include in relationships where meaningful (`:443`, `:5432`, `:9092`)
**Environments**: one diagram per environment, or annotate differences in a table
**Audience**: DevOps, SREs, Platform Engineers

---

## Dynamic (Sequence)

**Diagram type**: `sequenceDiagram`
**Participants**: `actor` for humans, `participant` for system elements
**Arrows**: `-->>` synchronous response, `-)` async / fire-and-forget
**Scope**: ONE flow or use case
**Happy path**: in the main diagram only
**Errors/edge cases**: in a dedicated section below the diagram, not inline
**Audience**: Developers, QA Engineers

---

## Universal Rules (all levels)

- Every node must have: label + type hint + 1-sentence description
- Every relationship must have a verb label
- L2 and below: every relationship must also include protocol/technology
- External systems always use `System_Ext` or external notation
- Never embed implementation detail in a higher-level diagram
- Stale or removed elements must be marked `[DEPRECATED]` during maintenance
