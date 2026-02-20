---
name: authoring-user-docs
description: "Use when producing user-facing documentation — tutorials, how-to guides, user guides, getting-started guides, installation guides, or onboarding documentation. Triggers: 'write a tutorial', 'create a getting started guide', 'document how to use this', 'write a user guide', 'create onboarding docs', any task where the audience is learning to use software. Always load authoring-technical-docs first."
---

# Authoring User Docs Action

Produces tutorials, how-to guides, and user guides — the learning-oriented and task-oriented quadrants of the Diátaxis framework.

**Load `authoring-technical-docs` first** for the multi-pass workflow, style rules, and quality framework. This action provides the templates and user-doc-specific rules.

---

## Choosing the right type

| | Tutorial | How-To Guide | User Guide |
|---|---|---|---|
| **Audience** | Beginners | Users who know the basics | All experience levels |
| **Structure** | Linear journey | Task-focused | Chapter-style, comprehensive |
| **Goal** | "I learned how this works" | "I accomplished my task" | "I understand the whole product" |

---

## Template: Tutorial

```markdown
---
title: "[Getting started with X / Build your first Y]"
description: "[One sentence: what the reader will learn and build]"
audience: beginner
doc-type: tutorial
estimated-time: [e.g., 15 minutes]
last-updated: [YYYY-MM-DD]
---

# [Title]

In this tutorial, you'll [what they'll accomplish]. By the end, you'll have [concrete outcome].

## Before you begin

Make sure you have:
- [Prerequisite 1 with version number]
- [Prerequisite 2 with link to installation guide]

## Step 1: [Action verb] [thing]

[Brief context: why this step matters — one sentence max.]

[Instruction using imperative mood.]

```language
[Code or command]
```

You should see [expected output or behavior].

## Step 2: [Action verb] [thing]

[Continue the pattern. Each step does ONE thing.]

> **Note:** [Any gotchas or important context for this step.]

## Verify it works

```language
[Verification command or test]
```

Expected output:

```
[What they should see]
```

## What you learned

- [Accomplishment 1]
- [Accomplishment 2]

## Next steps

- [Link to a more advanced tutorial]
- [Link to the relevant how-to guide]
```

---

## Template: How-to guide

```markdown
---
title: "How to [accomplish specific task]"
description: "[One sentence: the task this guide helps you complete]"
audience: [developer | admin | end-user]
doc-type: how-to
last-updated: [YYYY-MM-DD]
---

# How to [accomplish specific task]

This guide shows you how to [task]. Use this when you need to [situation].

## Prerequisites

- [Requirement with version]
- [Required access / permissions]

## Steps

### 1. [Action verb] [thing]

```language
[Code / command / UI instruction]
```

### 2. [Action verb] [thing]

### 3. [Action verb] [thing]

## Variations

### [Variation A]

[Alternative approach for a common variation.]

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| [Error message] | [Why it happens] | [How to fix it] |

## Related

- [Link to related guide]
- [Link to API reference]

## Templates

| User Doc | Asset |
|--------|--------|
| `User Guide` | Read `./assets/user_guide_template.md` |
| `Tutorial` | Read `./assets/tutorial_template.md` |
| `How-to guide` | Read `./assets/how_to_guide_template.md` |

## User docs rules

1. **One action per step.** Don't chain multiple actions in a single step.
2. **Show expected results.** After every significant step, tell the reader what they should see.
3. **Write prerequisites as a checklist.** Users verify each before starting.
4. **Never assume context.** Provide enough context in the first paragraph for someone arriving from a search engine.
5. **Progressive disclosure.** Most common path first. Edge cases in "Variations."
6. **End with next steps.** Always give the reader somewhere to go.
7. **Test every procedure.** If you can't follow it yourself, flag as a gap.

Save tutorials to `docs/guides/tutorial-*.md`. Save how-to guides to `docs/guides/how-to-*.md`. Save user guides to `docs/guides/guide-*.md`.


**Best Practices:**
- Use clear, non-technical language for end-users 
- Organize content logically with standardized structure 
- Include visual aids (screenshots, diagrams) where helpful 
- Keep instructions step-by-step and actionable 
**Key Principles:**
- **Progressive disclosure**: Start simple, add depth
- **Visual-first**: Every major action has a screenshot
- **Task-oriented**: Organize by what users want to achieve, not by UI layout
