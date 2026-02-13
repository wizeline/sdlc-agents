# Quality Framework Reference

This reference defines the quality standards, linting rules, and review criteria applied to all documentation produced by the technical-writer skill. The Critic subagent uses this as its primary rulebook.

## The Six Quality Dimensions

Every document is evaluated against these six dimensions. A document that excels in one but fails in another is not ready for delivery.

### 1. Accuracy

The document contains no factual errors. Every claim about the software's behavior is traceable to a source input (code, spec, PRD, or confirmed SME statement).

**How the Critic checks this:**
- For each factual claim, ask: "Which input artifact supports this?"
- If a claim has no traceable source, flag it as `[UNVERIFIED]`
- Pay special attention to: default values, error codes, parameter types, system requirements, compatibility claims

**Common accuracy failures:**
- Documenting planned behavior instead of implemented behavior
- Copying parameter names from an outdated spec
- Stating a feature "supports" something that is actually limited or beta

### 2. Completeness

Nothing important is missing. The document covers all parameters, all steps, all error cases, and all prerequisites.

**How the Critic checks this:**
- Compare the document against the research brief's inventory
- For API docs: every parameter, every response code, every error state
- For tutorials: every step needed to reach the stated outcome
- For release notes: every user-facing change in the release

**Common completeness failures:**
- Missing error handling section
- Undocumented optional parameters
- Tutorials that skip "obvious" steps (they're only obvious to the writer)
- Release notes that omit breaking changes

### 3. Usability

A reader at the stated skill level can accomplish their goal using only this document, without outside help.

**How the Critic checks this (First-Party Consumption Principle):**
- Read the document as if encountering the product for the first time
- For each step: "Do I know exactly what to do? Do I have enough context?"
- For each term: "Has this been defined or can I reasonably be expected to know it?"
- For each code example: "Could I copy-paste this and have it work?"

**Common usability failures:**
- Steps that assume knowledge not established earlier in the document
- Code examples with undefined variables or missing imports
- Instructions that reference UI elements without describing where they are
- "Configure the service" without explaining how or where

### 4. Consistency

The document uses the same term for the same thing throughout. Formatting follows a single pattern. The voice and tone don't shift.

**How the Critic checks this:**
- Build a terminology list while reading. Flag any term that appears in two forms (e.g., "API key" vs "api key" vs "API Key")
- Check heading style is uniform (all sentence case, or all title case — not a mix)
- Verify code examples use the same language/framework throughout unless explicitly showing alternatives
- Check that the same formatting convention is used for all UI elements (bold? quotes? code font?)

**Common consistency failures:**
- Calling the same thing different names ("workspace" in one section, "project" in another)
- Mixing code examples between `curl` and SDK calls without clear section breaks
- Headings that alternate between sentence case and title case

### 5. Readability

The text is clear, concise, and appropriate for the audience's reading level.

**How the Critic checks this:**
- Sentences longer than 25 words should be split
- Paragraphs longer than 5 sentences should be broken up
- Passive voice should be flagged (with specific rewrite suggestion)
- Jargon used without definition should be flagged
- Nested conditionals in instructions ("If X, then if Y, then if Z...") should be restructured

**Target reading levels:**
- End-user documentation: Flesch-Kincaid Grade Level ≤ 12
- Developer documentation: Flesch-Kincaid Grade Level ≤ 16
- These aren't hard limits — they're signals. If the score is too high, look for unnecessary complexity.

### 6. Structure

The document follows the correct template for its type and uses a logical information hierarchy.

**How the Critic checks this:**
- Heading levels are sequential (no jumping from H2 to H4)
- The document follows the appropriate template from the references
- Information flows from general to specific
- Prerequisites come before procedures
- Related content is linked, not duplicated
- Table of contents is present for documents with 4+ sections

## Style Linting Rules

These rules are checked automatically by the Critic. Each violation gets a severity rating.

### Blocker (must fix before delivery)

| Rule | Bad | Good |
|------|-----|------|
| Factual error | "Returns a 200 status" (when it returns 201) | "Returns a 201 Created status" |
| Missing prerequisite | Procedure assumes Node.js without stating it | "Prerequisites: Node.js 18 or later" |
| Broken code example | Example uses undefined variable | Complete, runnable example |
| Wrong audience | Developer jargon in end-user guide | Plain language appropriate to audience |

### Major (should fix)

| Rule | Bad | Good |
|------|-----|------|
| Passive voice | "The file is created by the system" | "The system creates the file" |
| Missing error handling | No troubleshooting section | Troubleshooting table with common errors |
| Undefined jargon | "Configure the CORS policy" | "Configure the CORS (Cross-Origin Resource Sharing) policy" |
| Ambiguous pronoun | "It processes the data and sends it to the server, which stores it" | "The client processes the data and sends the payload to the server. The server stores the payload in the database." |

### Minor (nice to fix)

| Rule | Bad | Good |
|------|-----|------|
| Missing Oxford comma | "logs, metrics and traces" | "logs, metrics, and traces" |
| Title case heading | "Setting Up Your Environment" | "Set up your environment" |
| Future tense | "The API will return..." | "The API returns..." |
| Third person | "The user can configure..." | "You can configure..." |

## The Gap Detection Protocol

The Critic must actively look for contradictions and missing information across sources. This is one of the highest-value activities in the review.

**Types of gaps:**

1. **Spec-code mismatch:** The spec says one thing, the code does another. Flag with: "Spec states [X], but code analysis suggests [Y]. Verify with engineering."
2. **Missing information:** A feature is mentioned in the PRD but has no corresponding API endpoint or UI element. Flag with: "Feature [X] is referenced in [source] but not found in [other source]."
3. **Stale content:** Documentation references a version, endpoint, or feature that no longer exists. Flag with: "Reference to [X] may be outdated. Last verified: [date]."
4. **Implicit assumptions:** The document assumes knowledge that isn't established. Flag with: "Step [N] assumes the reader knows [X], which is not explained anywhere in this document or its prerequisites."

**When gaps are found:**
- Do NOT invent information to fill the gap
- Mark the gap clearly with `[GAP: description]` inline
- Add it to the review summary for human resolution
- If possible, suggest where the missing information might be found

## Document Health Metrics

For ongoing documentation maintenance, track these metrics:

| Metric | What it measures | How to check |
|--------|-----------------|--------------|
| **Link integrity** | All internal and external links resolve | Crawl all links, flag 404s |
| **Freshness** | Docs updated within an acceptable window | Check `last-updated` frontmatter |
| **Coverage** | All public APIs/features have documentation | Compare API surface to docs inventory |
| **Example validity** | Code examples are syntactically correct | Parse code blocks, check for obvious errors |
| **Terminology consistency** | Same terms used across all docs | Build a term frequency index, flag variants |
