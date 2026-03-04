# C4 Anti-Patterns

Common mistakes to detect and refuse to produce.

---

## Anti-Pattern Catalogue

| Anti-Pattern | Symptom | Correct Approach |
|-------------|---------|-----------------|
| **Level mixing** | Components shown inside an L2 diagram | Strict per-level scope — move to L3 |
| **Unlabeled arrows** | Relationships with no text | Every `Rel` requires a verb label |
| **Missing descriptions** | Empty string in node description | Every node gets a 1-sentence description |
| **Missing technology (L2+)** | `Container(api, "API", "", "")` | Every container shows its tech stack |
| **Missing protocol (L2+)** | `Rel(a, b, "calls")` with no protocol | Add `[REST/JSON]`, `[gRPC]`, `[SQL]`, etc. |
| **Mega-diagram** | >20 nodes, unreadable | Split by bounded context or service group |
| **God container** | One "Backend" box for all logic | Decompose into real containers |
| **1-file = 1-component** | 40 components in an L3 diagram | Group by responsibility, not file |
| **Infra in L2** | k8s nodes or VMs inside Container diagram | Move infra to Deployment diagram |
| **Components in Deployment** | Class detail in infra diagram | Stay at container level in Deployment |
| **Error paths in main diagram** | Sequence diagram with alt blocks everywhere | Happy path only in main; errors in a section |
| **Stale technology** | "Java 8", "React 16" in a current diagram | Verify and update tech versions |
| **Missing boundary** | All elements at same visual level | Use `System_Boundary` / `Container_Boundary` |
