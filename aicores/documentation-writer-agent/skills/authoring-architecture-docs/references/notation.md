# C4 Notation & Definitions

Source: https://c4model.com

---

## Abstractions

| Abstraction | Definition |
|-------------|------------|
| **Person** | A human user (internal or external) who interacts with the system |
| **Software System** | The highest-level abstraction; a system that delivers value to users |
| **Container** | A separately deployable/runnable unit: app, service, DB, queue, store |
| **Component** | A logical grouping of related functionality inside a container |
| **Code** | Implementation detail: classes, interfaces, functions |

---

## Diagram Levels

| Level | Name | Zooms Into | Shows |
|-------|------|------------|-------|
| L1 | System Context | — | System + Persons + External Systems |
| L2 | Container | Software System | Deployable units + their tech + comms |
| L3 | Component | One Container | Internal logical components + their roles |
| L4 | Code | One Component | Classes, interfaces, ER model |
| — | Deployment | Infrastructure | Where containers run |
| — | Dynamic | A specific flow | Sequence of interactions over time |

---

## Mermaid Element Types

```
Person(alias, "Label", "Description")
System(alias, "Label", "Description")
System_Ext(alias, "Label", "Description")
System_Boundary(alias, "Label") { ... }

Container(alias, "Label", "Technology", "Description")
ContainerDb(alias, "Label", "Technology", "Description")
ContainerQueue(alias, "Label", "Technology", "Description")
Container_Boundary(alias, "Label") { ... }

Component(alias, "Label", "Type: Technology", "Description")

Deployment_Node(alias, "Label", "Technology") { ... }

Rel(from, to, "Label", "[Technology]")
Rel_Back(from, to, "Label", "[Technology]")
```

---

## Component Types (L3)

| Type | Responsibility |
|------|---------------|
| Controller / Handler | Accepts incoming requests; delegates to services |
| Service / UseCase | Encapsulates business logic |
| Repository / DAO | Abstracts data persistence |
| Gateway / Client | Wraps calls to external systems |
| EventPublisher | Emits domain events to a queue/topic |
| EventConsumer | Reacts to incoming domain events |
| Scheduler | Triggers time-based operations |

---

## Relationship Verb Bank

| Scenario | Use |
|----------|-----|
| User opens UI | "Visits", "Uses" |
| Service calls another | "Calls", "Requests from" |
| Service reads data | "Reads from", "Queries" |
| Service writes data | "Writes to", "Persists to" |
| Publishes to queue | "Publishes to", "Emits event to" |
| Consumes from queue | "Consumes from", "Subscribes to" |
| Auth | "Authenticates via", "Delegates auth to" |
| Notification | "Sends email via", "Sends SMS via" |
| File access | "Stores in", "Retrieves from" |
