# Release Documentation Reference

This reference covers release notes, changelogs, and READMEs — time-sensitive and high-visibility documentation.

## Template: Release Notes

```markdown
---
title: [Product name] [version] release notes
description: What's new, improved, and fixed in [version]
date: [YYYY-MM-DD]
---

# [Product name] [version] release notes

*Released [Month Day, Year]*

[One paragraph summary: the headline change in this release and who benefits from it.]

## New features

### [Feature name]

[2-3 sentences: what it does and why it matters. Link to the full documentation.]

### [Feature name]

[Same pattern.]

## Improvements

- **[Area]:** [What improved and the user-visible effect.]
- **[Area]:** [What improved and the user-visible effect.]

## Bug fixes

- Fixed an issue where [specific behavior] occurred when [trigger condition]. ([#ticket-id])
- Fixed [description]. ([#ticket-id])

## Breaking changes

> **Action required:** [Describe exactly what users need to do to migrate.]

- **[Change]:** [What changed, why, and the migration path.]

## Deprecations

- **[Feature/API]** is deprecated and will be removed in [version]. Use [replacement] instead. See the [migration guide](link).

## Known issues

- [Description of known issue and workaround if available.]

## Upgrade instructions

[Step-by-step instructions to upgrade from the previous version. Include commands.]
```

## Template: Changelog (KEEP A CHANGELOG format)

```markdown
# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- [New feature or capability]

### Changed
- [Modification to existing functionality]

### Deprecated
- [Feature marked for removal]

### Removed
- [Feature that was removed]

### Fixed
- [Bug fix]

### Security
- [Security-related fix]

## [1.2.0] - 2026-01-15

### Added
- [Feature description]

### Fixed
- [Bug fix description]

[1.2.0]: https://github.com/org/repo/compare/v1.1.0...v1.2.0
[Unreleased]: https://github.com/org/repo/compare/v1.2.0...HEAD
```

## Template: README

```markdown
# [Project name]

[One sentence: what this project does.]

[One sentence: why someone would use it — the key benefit.]

## Quick start

```bash
[Minimal installation command]
```

```language
[Smallest working example — 5 lines or fewer]
```

## Installation

### Requirements

- [Runtime] >= [version]
- [Dependency] >= [version]

### Install via [package manager]

```bash
[Install command]
```

### Install from source

```bash
git clone [repo-url]
cd [project-name]
[build command]
```

## Usage

[The 2-3 most common use cases, each with a code example.]

### [Use case 1]

```language
[Example]
```

### [Use case 2]

```language
[Example]
```

## Configuration

[Key configuration options. Link to full reference if long.]

| Option | Description | Default |
|--------|-------------|---------|
| `option` | What it does | `default` |

## Documentation

- [Link to full docs site]
- [Link to API reference]
- [Link to tutorials]

## Contributing

[Brief instructions or link to CONTRIBUTING.md]

## License

[License name]. See [LICENSE](LICENSE) for details.
```

## Rules Specific to Release Docs

1. **Lead with impact, not implementation.** "You can now export dashboards as PDF" not "Added PDF rendering pipeline to the export module."
2. **Be specific in bug fixes.** "Fixed an issue where CSV exports truncated rows with more than 50 columns" not "Fixed export bug."
3. **Breaking changes go in a callout box.** They must be impossible to miss. Include the exact migration steps.
4. **Link to tickets.** Every bug fix and feature should reference its issue tracker ID.
5. **Use past tense for changes.** "Added", "Fixed", "Removed" — not "Adds", "Fixes", "Removes."
6. **Include dates.** Every release has a date. Every deprecation has a removal target date.
7. **READMEs must work standalone.** A developer finding the repo for the first time should be able to install and try the project using only the README.
8. **Keep the quick start under 30 seconds.** If your quick start takes longer, simplify it.

## Generating Release Notes from Inputs

When given Jira exports, commit logs, or ticket lists:

1. **Group by type:** features, improvements, bug fixes, breaking changes
2. **Rewrite for users:** Commit messages are written for developers. Release notes are for users. Translate "refactor auth middleware" to "Improved login reliability."
3. **Prioritize by impact:** Most important changes first within each section
4. **Cross-reference:** If a bug fix relates to a feature, mention the connection
5. **Flag anything unclear:** If a ticket description is too vague to write a user-facing note, flag it for human review rather than guessing
