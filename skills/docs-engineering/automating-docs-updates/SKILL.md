---
name: automating-docs-updates
description: "Automatically handles documentation updates whenever a user requests to commit and push changes through Git. Reads the changes being committed and updates the relevant documentation accordingly."
context: fork
agent: Plan
---

# Automating Docs Updates on Commit

## Overview

This skill ensures that documentation stays synchronized with the codebase by automatically intercepting commit/push workflows. Whenever a user is about to commit and push changes via Git, this skill analyzes the changes and updates the relevant documentation before the commit is finalized.

## Workflow

1. **Analyze Changes**: Read the changes being committed (e.g., via `git diff --cached` or viewing the modified files).
2. **Identify Relevant Documentation**: Determine which documentation files (API docs, architecture overviews, user guides, or the README) cover the code that was changed.
3. **Update Documentation**: Apply the necessary updates to the documentation using the standard `authoring-technical-docs` guidelines.
4. **Stage Updates**: Stage the newly updated documentation files so they are included in the same commit.
5. **Proceed with Commit/Push**: Once the docs are updated and staged, the normal commit and push process can proceed.

## Guidelines

- **Completeness**: Make sure all affected endpoints, functions, and architectural components are updated in the docs.
- **Accuracy**: The updated docs must accurately reflect the code changes.
- **Integration**: Works in tandem with other `docs-engineering` skills like `authoring-api-docs` or `authoring-release-docs` depending on what was changed.
