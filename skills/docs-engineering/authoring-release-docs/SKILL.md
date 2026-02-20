---
name: authoring-release-docs
description: "Use when producing release-oriented documentation — release notes, changelogs, READMEs, or migration guides. Triggers: 'write release notes', 'generate a changelog', 'create a README', 'document what changed', 'write migration guide', any task involving Jira exports, commit logs, or ticket lists that need to become user-facing documentation. Always load authoring-technical-docs first."
---

# Authoring Release Docs Action

Produces release notes, changelogs, and READMEs — time-sensitive, high-visibility documentation.

**Load `authoring-technical-docs` first** for the multi-pass workflow, style rules, and quality framework. This action provides the templates and release-specific rules.

---

## Template: Release notes

```markdown
---
title: "[Product name] [version] release notes"
description: "What's new, improved, and fixed in [version]"
doc-type: reference
date: [YYYY-MM-DD]
---

# [Product name] [version] release notes

*Released [Month Day, Year]*

[One paragraph: the headline change and who benefits.]

## New features

### [Feature name]

[2-3 sentences: what it does and why it matters. Link to full docs.]

## Improvements

- **[Area]:** [What improved and the user-visible effect.]

## Bug fixes

- Fixed an issue where [specific behavior] occurred when [trigger condition]. ([#ticket-id])

## Breaking changes

> **Action required:** [Exactly what users need to do to migrate.]

- **[Change]:** [What changed, why, and the migration path.]

## Deprecations

- **[Feature/API]** is deprecated and will be removed in [version]. Use [replacement] instead.

## Known issues

- [Description of known issue and workaround if available.]

## Upgrade instructions

[Step-by-step to upgrade from previous version. Include commands.]
```

---

## Template: Changelog (Keep a Changelog format)

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

### Fixed
- [Bug fix]

### Security
- [Security-related fix]

## [1.2.0] - 2026-01-15

### Added
- [Feature description]

[1.2.0]: https://github.com/org/repo/compare/v1.1.0...v1.2.0
[Unreleased]: https://github.com/org/repo/compare/v1.2.0...HEAD
```

---

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

### Install via [package manager]

```bash
[Install command]
```

## Usage

### [Use case 1]

```language
[Example]
```

## Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `option` | What it does | `default` |

## Documentation

- [Link to full docs]
- [Link to API reference]

## Contributing

[Brief instructions or link to CONTRIBUTING.md]

## License

[License name]. See [LICENSE](LICENSE).
```

---

## Release docs rules

1. **Lead with impact, not implementation.** "You can now export dashboards as PDF" — not "Added PDF rendering pipeline."
2. **Be specific in bug fixes.** "Fixed an issue where CSV exports truncated rows with more than 50 columns" — not "Fixed export bug."
3. **Breaking changes go in a callout box.** They must be impossible to miss. Include exact migration steps.
4. **Link to tickets.** Every bug fix and feature references its issue tracker ID.
5. **Use past tense for changes.** "Added", "Fixed", "Removed."
6. **Include dates.** Every release has a date. Every deprecation has a removal target date.
7. **READMEs must work standalone.** A developer finding the repo for the first time can install and run the project using only the README.
8. **Keep the quick start under 30 seconds.**

---

## Generating release notes from inputs

When given Jira exports, commit logs, or ticket lists:

1. **Group by type**: features, improvements, bug fixes, breaking changes
2. **Rewrite for users**: commit messages are for developers, release notes are for users
3. **Prioritize by impact**: most important changes first within each section
4. **Flag anything unclear**: if a ticket is too vague, flag for human review rather than guessing

Save release notes to `docs/releases/v[version].md`. Save changelog to `CHANGELOG.md` at project root. Save README to project root.
