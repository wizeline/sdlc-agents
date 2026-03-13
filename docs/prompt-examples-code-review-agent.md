# Code Review Agent — Prompt Examples

Ready-to-use prompts for the **Code Review** AI Core.
This core is powered by the **code-reviewer** agent and **6 specialized skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically trigger its review pipeline, running specialists in parallel to produce a consolidated report.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
| --- | --- |
| `code-reviewer` | `code-review-securing`, `code-review-optimizing`, `code-review-validating`, `code-review-refactoring`, `code-review-contextualizing`, `code-review-orchestrating` |

---

## Skill Outputs Quick Reference

The agent produces a consolidated report by default, but each specialist also saves its own focused report.

| Output Type | Produced By | What It Contains |
| --- | --- | --- |
| **Consolidated Report** | `code-review-orchestrating` | Unified view of all dimensions, de-duplicated findings, and a weighted overall score. |
| **Security Report** | `code-review-securing` | Deep-dive into vulnerabilities (SQLi, XSS, Auth leaks) with CWE mapping and before/after fixes. |
| **Performance Report** | `code-review-optimizing` | Analysis of N+1 queries, algorithmic complexity (O(n²)), memory leaks, and I/O bottlenecks. |
| **Correctness Report** | `code-review-validating` | Identification of race conditions, null dereferences, edge cases, and business logic flaws. |
| **Refactoring Report** | `code-review-refactoring` | Suggestions for clean code, design pattern implementation, and complexity reduction. |

---

## 🎯 Intent-Based Triggers (Standalone Skills)

You do not need to explicitly call `@code-reviewer`. Use these natural phrases to trigger specific specialized skills.

### 🔒 Security Audit (`code-review-securing`)

- "Audit this Express.js middleware for injection risks and OWASP Top 10 vulnerabilities."
- "Check for hardcoded secrets or insecure configuration patterns in these files."
- "Trace user input from this API endpoint to see if it reaches any dangerous sinks."

### ⚡ Performance Optimization (`code-review-optimizing`)

- "Find performance bottlenecks in this processing loop and suggest optimizations."
- "Analyze this DB repository for N+1 query problems or missing indexes."
- "Can we make this batch worker more efficient with better parallelism/concurrency?"

### ✔️ Logical Correctness (`code-review-validating`)

- "Look for potential race conditions or deadlocks in this concurrency logic."
- "Review this payment handler for edge cases—what happens on network failure or null inputs?"
- "Check these business rules for off-by-one errors or logical inconsistencies."

### 🏗️ Code Quality & Refactoring (`code-review-refactoring`)

- "Check for code smells and technical debt in this module."
- "Does this code follow SOLID principles? Suggest refactors to reduce coupling."
- "Audit the documentation coverage and quality for these public functions."

---

## 🤖 `code-reviewer` — Orchestrated Reviews

> **Persona:** Developer wanting a comprehensive check before merging or committing.

### Full Pipeline Prompts

- "Perform a full code review on `src/services/auth-service.ts`."
- "Review the contents of the `src/api/v1/` directory."
- "Review my staged changes (`git diff --cached`). Does this block the commit?"

### Persona-Based Triggers (No Handle Needed)

- "Act as an automated code review bot and generate a gate decision report for this PR."
- "Perform a comprehensive pre-merge audit of `src/auth/` and save the consolidated report."

---

## 🌪️ The "Global Review" Super-Prompts

Use these when you need the entire AI core to evaluate a complex scenario from every angle.

### 1. The Comprehensive Super-Prompt (All Agents & Skills)

```text
Review this entire feature branch for production-readiness. 
1. First, map the codebase context to identify all callers and transitive dependencies. 
2. Run parallel specialist audits for security vulnerabilities (ASVS Level 2), performance bottlenecks (N+1, O(n²) complexity), logical correctness (race conditions, edge cases), and maintainability (SOLID, DRY). 
3. Synthesize everything into a consolidated action report ordered by severity, compute a weighted score, and provide a final gate decision.
```

### 2. The Short Version (Concise Trigger)

```text
Perform a context-aware, 4-dimension code review (Security, Perf, Correctness, Refactor) on this PR. Deliver a consolidated report with a gate decision and prioritized fixes.
```

---

## Tips for Better Results

1. **Be specific about the "What"**: Instead of "review this", say "check this for memory leaks" or "audit this for JWT security".
2. **Define the Gate**: If you want a pass/fail decision, include terms like "gate decision", "merge block", or "production-ready check".
3. **Data Sensitivity**: Mention if the code handles sensitive data like PII or financial records; the agent will escalate finding severities accordingly.
4. **Mention Standards**: Referencing "OWASP", "ASVS", "SOLID", or "DRY" immediately signals the specialist skill needed.
