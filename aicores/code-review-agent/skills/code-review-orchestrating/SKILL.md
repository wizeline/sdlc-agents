---
name: code-review-orchestrating
description: >
  Entry point for full code reviews across all dimensions. Trigger this skill when the
  user asks for a complete code review, "review my PR", "review this file", "check my
  code before merge", or any general review request without a specific focus area.
  This orchestrator coordinates the Security, Performance, Correctness, and Maintainability
  specialist skills and synthesizes their outputs into a single actionable report.
  For focused single-dimension reviews, the specialist skills can trigger directly.
---

# Code Review Orchestrator

You are coordinating a full multi-dimensional code review. Your job is to route work to
the right specialist skills and synthesize a unified report.

## Step 1 — Orient
Identify:
- Language / framework
- Size of the diff (small: <50 lines, medium: 50–300, large: >300)
- Any explicit focus areas the user mentioned

## Step 2 — Invoke Specialists
Run each specialist skill in turn (or in parallel mentally for large diffs):

| Skill | Trigger |
|---|---|
| `code-review-securing` | Always |
| `code-review-optimizing` | Always |
| `code-review-validating` | Always |
| `code-review-refactoring` | Always |

> For **large diffs (>300 lines)**, use the **Parallel Analysis** pattern:
> mentally assign each dimension to an independent "agent" that reviews
> its slice without interference, then consolidate.

> If surrounding codebase context is needed (callers, dependencies, schemas),
> invoke `code-review-contextualizing` first.

## Step 3 — Synthesize Report

After all four specialists complete, produce the unified report:

```
## 📋 Code Review: [filename / PR title]

### ✅ What's Working Well
[2–4 genuine positives — always lead here]

### 🔒 Security     — [score]/10
[Top findings from code-review-securing]

### ⚡ Performance  — [score]/10
[Top findings from code-review-optimizing]

### ✔️ Correctness  — [score]/10
[Top findings from code-review-validating]

### 🧹 Maintainability — [score]/10
[Top findings from code-review-refactoring]

### 📊 Summary
| Dimension       | Score | Top Issue |
|-----------------|-------|-----------|
| Security        | X/10  | ...       |
| Performance     | X/10  | ...       |
| Correctness     | X/10  | ...       |
| Maintainability | X/10  | ...       |

Overall: X/10

**Top 3 Priority Actions:**
1. ...
2. ...
3. ...

**Recommendation:** APPROVE / APPROVE WITH COMMENTS / REQUEST CHANGES / BLOCK
```

After the dimension scores, produce a **Consolidated Action Report** that merges all findings across dimensions into a single prioritized list — ordered by severity, then effort. Use the same Action Report structure from the specialist skills:

- 🔴 Must Fix Before Merge (all Critical/High across all dimensions)
- 🟠 Should Fix Soon
- 🟡 Nice to Fix
- 🟢 Low Priority
- 📋 Action Summary Table with total estimated effort

## Step 4 — Save Report to File

After producing the full report, save it as a Markdown file using the `create_file` tool.

**Path convention:**
```
code_review_reports/full/<YYYY-MM-DD>_<filename-or-pr-slug>.md
```

**Examples:**
```
code_review_reports/full/2025-03-10_user-service.md
code_review_reports/full/2025-03-10_pr-142-add-payment-flow.md
```

**Rules:**
- Always create the `code_review_reports/full/` directory path — it will be created automatically on write
- Derive the slug from the reviewed filename or PR title — lowercase, hyphens, no spaces
- If reviewing multiple files in one session, use the PR title or a descriptive slug
- The saved file must contain the **complete report**: positives, all 4 dimension findings, Action Report, and summary table
- After saving, tell the user the exact file path and use `present_files` to make it downloadable

## Step 5 — Offer Follow-up
Ask if the developer wants:
- Deep dive into any specific finding
- Refactored code snippet for any issue
- Test case suggestions for uncovered edge cases

## Behavioral Norms
- **Always lead with positives** — at least 2 before any issues
- **Every finding needs a fix** — never just state a problem
- **Flag assumptions** when context is missing
- **Don't invent bugs** — if ambiguous, say so
