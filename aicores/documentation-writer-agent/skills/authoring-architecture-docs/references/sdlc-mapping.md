# SDLC Phase → C4 Diagram Mapping

---

## Phase Matrix

| SDLC Phase | Produce | Audience |
|------------|---------|----------|
| Requirements | L1 System Context | Stakeholders, PMs |
| Design | L1 + L2 Container | Architects, Tech Leads |
| Development | L2 + L3 Component (per service) | Developers |
| Testing | Dynamic (Sequence) | Developers, QA |
| Deployment | Deployment Diagram | DevOps, SREs |
| Maintenance | All — kept current | Entire team |

---

## Audience Decision Tree

```
Who needs this?
├── Executive / PM / Stakeholder  → L1 only
├── Architect / Tech Lead         → L1 + L2
├── Developer (new to system)     → L2 + L3
├── Developer (on one service)    → L3 (+ L4 if logic is complex)
├── QA / Tester                   → Dynamic
└── DevOps / SRE                  → Deployment

What input exists?
├── Description only              → L1
├── Tech stack known              → L1 + L2
├── Source code available         → L2 + L3 + L4
├── IaC / k8s / compose files     → Deployment
└── API traces / flow description → Dynamic
```

---

## Artifact Lifecycle

Diagrams are **refined** as the project matures — not replaced.

```
Requirements  →  L1 created
Design        →  L1 refined  +  L2 created
Development   →  L2 refined  +  L3 created per service
Testing       →               +  Dynamic created per key flow
Deployment    →               +  Deployment diagram created
Maintenance   →  All kept current; deprecated elements marked [DEPRECATED]
```
