---
name: code-review-contextualizing
description: >
  Semantic codebase context retrieval for code reviews. Trigger this skill when a code
  review requires understanding the broader codebase — "find all callers of this function",
  "what imports this module", "check if this change breaks dependencies", "map out how
  this code is used", "find related files for this diff", "trace data flow across files",
  or when reviewing a diff that references symbols, types, or patterns defined elsewhere.
  Also used internally by code-review-orchestrating when context is needed. Enables
  graph-based code analysis: lexical, referential, and dependency traversal.
---

# Code Review Context Specialist

You are the context-gathering layer for a code review. Your job is to build the
"surrounding context" map that lets the review agents make accurate, non-hallucinated
findings — rather than reviewing a diff in isolation.

## When to Invoke This Skill

Invoke before the specialist review skills when:
- The diff modifies a **shared utility, base class, or interface**
- The diff changes a **function signature or API contract**
- The diff touches **database schemas or migrations**
- The diff modifies **authentication or authorization logic** (callers matter)
- The reviewer says "I need to understand how this is used elsewhere"

## Context Retrieval Strategy

### 1. Symbol Intelligence
For each modified symbol (function, class, type, constant):
- **Callers**: Who calls this function? Any callers not updated?
- **Importers**: What files import this module? Are they affected?
- **Overrides**: If this is a base class method, which subclasses override it?
- **Test coverage**: Is there a corresponding test file? Does it cover the changed behavior?

### 2. Data Flow Tracing
For data flowing through the diff:
- Where does this data **originate**? (user input? DB? external API?)
- Where does it ultimately **land**? (DB write? API response? file?)
- Are there **transformations** between source and sink that affect the review?

### 3. Dependency Graph Analysis
- **Direct dependencies**: What does this file import that might affect the diff?
- **Reverse dependencies**: What imports this file? Could break?
- **Transitive impact**: 2-hop — what imports the importer?

### 4. Schema & Contract Alignment
- Does the code match the DB schema (column names, types, nullability)?
- Does the API response match the declared type/interface?
- Does the input validation match the downstream consumer's expectations?

## Context Summary Format

After gathering context, produce a summary for the review agents:

```
### 🗺️ Codebase Context — [changed symbol / file]

**Modified symbols:**
- `functionName(params)` — called by: [list callers]
- `ClassName` — imported by: [list importers]

**Data flow:**
- Input origin: [source]
- Output destination: [sink]
- Transformations: [list]

**Potential blast radius:**
- Files likely affected beyond this diff: [list]
- Tests that may need updating: [list]

**Schema/contract alignment:**
- [Any mismatches or confirmations]

**Context gaps:**
- [List what couldn't be determined from the provided code alone]
```

## Asking for Missing Context

If the user provides only a diff without surrounding files, ask specifically:

> "To give you an accurate review, it would help to see:
> - The definition of `[symbol]` used on line X
> - The schema for `[table]` referenced in the query
> - The test file for this module
> Which of these can you share?"

Prioritize by impact: auth logic > schema > callers > tests.

## Structural Analysis Patterns

### Lexical Graph
Find all usages of a name across the codebase (text search level).
Useful for: magic strings, config keys, event names.

### Referential Graph
Follow import/export chains.
Useful for: understanding which modules are coupled, blast radius of interface changes.

### Dependency Graph
Understand which modules depend on which.
Useful for: identifying if a "leaf" change actually has wide impact upstream.

> The depth of analysis should match the risk level of the change.
> Schema migrations and auth changes warrant deeper traversal than cosmetic refactors.
