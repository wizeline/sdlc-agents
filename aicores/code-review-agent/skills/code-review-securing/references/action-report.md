# Action Report Template

Every specialist review must close with an **Action Report** section. This is the
developer-facing deliverable: prioritized, unambiguous, and ready to hand off to a
ticket or a PR comment.

---

## Action Report Structure

```
---
## 🎯 Action Report — [Dimension] — [filename]

### 🔴 Must Fix Before Merge
[Only include if Critical/High findings exist. These block the PR.]

**Action 1 — [Short title, e.g. "Parameterize the SQL query on L42"]**
- **File:** `path/to/file.py`
- **Location:** Line 42, function `get_user()`
- **What to do:** Replace string concatenation with a parameterized query
- **Why it matters:** SQL injection — attacker can exfiltrate the entire users table
- **Code:**
  ```python
  # Before
  cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")

  # After
  cursor.execute("SELECT * FROM users WHERE id = %s", [user_id])
  ```
- **Estimated effort:** 5 min
- **Verify by:** Running `sqlmap` against the endpoint, or checking that no string interpolation reaches a DB call

---

**Action 2 — [Next blocking item]**
[same structure]

---

### 🟠 Should Fix Soon
[High-priority but not blocking. Target: next sprint.]

**Action 3 — [Short title]**
- **File / Location / What / Why / Code / Effort / Verify by**

---

### 🟡 Nice to Fix
[Medium findings. Target: backlog.]

**Action 4 — [Short title]**
- **What to do:** [one sentence]
- **Location:** [file + line]

---

### 🟢 Low Priority / Optional
[Low findings. List only — no full action block needed.]

- L19: Extract `86400` → `SECONDS_PER_DAY = 86_400`
- L67: Add `#1234` ticket ref to TODO comment

---

### 📋 Action Summary Table
| # | Priority | Action | Location | Effort |
|---|----------|--------|----------|--------|
| 1 | 🔴 Must Fix | Parameterize SQL query | `get_user()` L42 | 5 min |
| 2 | 🔴 Must Fix | Add auth middleware to `/admin` | `routes.py` L78 | 15 min |
| 3 | 🟠 Should Fix | Add rate limiting to login endpoint | `auth.py` L12 | 30 min |
| 4 | 🟡 Nice to Fix | Extract magic number | `utils.py` L19 | 2 min |

**Total estimated effort:** ~52 min
```

---

## Rules for Writing Action Items

1. **One action = one change.** Don't bundle multiple fixes into one action.
2. **"What to do" must be a verb phrase** — not a description of the problem.
   - ❌ "SQL injection vulnerability in query builder"
   - ✅ "Replace string concatenation with parameterized query in `build_query()` L42"
3. **Always include a Before/After code snippet** for Critical and High findings.
4. **Effort estimate is required** for Must Fix and Should Fix items.
   Use: `< 5 min` / `~15 min` / `~30 min` / `~1 hr` / `> 1 hr`
5. **"Verify by"** tells the developer how to confirm the fix worked —
   a test to run, a behavior to observe, or a tool to check with.
6. **Low-priority items** go in the summary table only — no full action block.

---

## Effort Estimation Guide

| Change Type | Typical Effort |
|---|---|
| Rename variable / extract constant | < 5 min |
| Add null guard or error check | 5–10 min |
| Swap dangerous call for safe alternative | 5–15 min |
| Add docstring / JSDoc to a function | 5–10 min |
| Refactor function into 2–3 smaller ones | 30–60 min |
| Add auth middleware to a route | 10–20 min |
| Fix N+1 query with eager loading | 15–30 min |
| Add DB index | 10 min (+ migration) |
| Refactor God class | > 1 hr |
