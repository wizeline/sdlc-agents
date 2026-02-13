# User Documentation Reference

This reference covers tutorials, how-to guides, and user guides — the learning-oriented and problem-oriented quadrants of the Diátaxis framework.

## Choosing Between Tutorials and How-To Guides

| | Tutorial | How-To Guide |
|---|---|---|
| **Audience** | Beginners learning for the first time | Users who already know the basics |
| **Structure** | Linear, step-by-step journey | Task-focused, can be read independently |
| **Tone** | Encouraging, explanatory | Direct, efficient |
| **Goal** | "I learned how this works" | "I accomplished my specific task" |
| **Length** | Longer — includes context | Shorter — just the steps |
| **Examples** | Walk through one complete scenario | Cover variations and edge cases |

## Template: Tutorial

```markdown
---
title: [Getting started with X / Build your first Y]
description: [One sentence: what the reader will learn and build]
audience: [beginner / intermediate]
estimated-time: [e.g., 15 minutes]
last-updated: [YYYY-MM-DD]
---

# [Title]

In this tutorial, you'll [what they'll accomplish]. By the end, you'll have [concrete outcome].

## Before you begin

Make sure you have:
- [Prerequisite 1 with version number]
- [Prerequisite 2 with link to installation guide]
- [Any account setup or access requirements]

## Step 1: [Action verb] [thing]

[Brief context: why this step matters — one sentence max.]

[Instruction using imperative mood.]

```language
[Code or command]
```

You should see [expected output or behavior].

## Step 2: [Action verb] [thing]

[Continue the pattern. Each step should do ONE thing.]

> **Note:** [Any gotchas or important context for this step.]

## Step 3: [Action verb] [thing]

[Steps build on each other. Reference previous steps by number when needed.]

## Verify it works

[A clear way for the reader to confirm everything is working.]

```language
[Verification command or test]
```

Expected output:

```
[What they should see]
```

## What you learned

In this tutorial, you:
- [Accomplishment 1]
- [Accomplishment 2]
- [Accomplishment 3]

## Next steps

- [Link to a more advanced tutorial]
- [Link to the relevant how-to guide for common tasks]
- [Link to the API reference for deeper customization]
```

## Template: How-To Guide

```markdown
---
title: How to [accomplish specific task]
description: [One sentence: the task this guide helps you complete]
audience: [developer / admin / end-user]
last-updated: [YYYY-MM-DD]
---

# How to [accomplish specific task]

This guide shows you how to [task]. Use this when you need to [situation where this is useful].

## Prerequisites

- [Requirement with version]
- [Required access / permissions]
- [Link to setup if needed]

## Steps

### 1. [Action verb] [thing]

[Direct instruction. No unnecessary context.]

```language
[Code / command / UI instruction]
```

### 2. [Action verb] [thing]

[Continue with minimal explanation. The reader knows the basics.]

### 3. [Action verb] [thing]

[Complete the task.]

## Variations

### [Variation A: e.g., "Using environment variables instead"]

[Alternative approach for a common variation.]

### [Variation B: e.g., "For Windows users"]

[Platform-specific or context-specific alternative.]

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| [Error message or symptom] | [Why it happens] | [How to fix it] |
| [Error message or symptom] | [Why it happens] | [How to fix it] |

## Related

- [Link to related how-to guide]
- [Link to explanation of underlying concept]
- [Link to API reference]
```

## Template: User Guide (Comprehensive)

For longer, chapter-style documentation:

```markdown
---
title: [Product / Feature] user guide
description: [One sentence describing what this guide covers]
audience: [end-user / admin / developer]
last-updated: [YYYY-MM-DD]
---

# [Product / Feature] user guide

## Overview

[2-3 paragraphs: what this product/feature does, who it's for, and what problems it solves. No jargon.]

## Key concepts

[Define the 3-5 most important terms/concepts the reader needs to understand. Use a definition list or short paragraphs.]

- **[Term 1]** — [Plain-language definition]
- **[Term 2]** — [Plain-language definition]

## Getting started

[Quick-start path: the minimum steps to see value. Link to the full tutorial for details.]

## [Feature area 1]

### [Sub-feature or task]

[Instructions, organized by task rather than by UI element.]

## [Feature area 2]

### [Sub-feature or task]

[Continue for each major area.]

## Configuration

[All configurable options, with defaults and descriptions.]

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `option_name` | string | `"default"` | What this option controls. |

## Frequently asked questions

**Q: [Common question]**
A: [Direct answer. Link to relevant section if more detail is needed.]

## Troubleshooting

[Common issues and solutions. Use the table format from the how-to template.]

## Glossary

[If the document introduces many terms, collect them here alphabetically.]
```

## Rules Specific to User Docs

1. **Test every procedure yourself.** Before writing it, actually do it. If you can't do it, flag it as a gap.
2. **One action per step.** "Click Settings, then select Privacy, then toggle Location Services off" should be three steps, not one.
3. **Show expected results.** After every significant step, tell the reader what they should see. This confirms they're on track.
4. **Include screenshots sparingly.** Only when the UI is genuinely confusing. Annotate with numbered callouts, not arrows.
5. **Write prerequisites as a checklist.** Users should be able to verify each one before starting.
6. **Never assume context.** If a reader might jump into this page from a search engine, provide enough context in the first paragraph.
7. **Use progressive disclosure.** Put the most common path first. Tuck edge cases into collapsible sections or "Variations."
8. **End with next steps.** Always give the reader somewhere to go after finishing.
