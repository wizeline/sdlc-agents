---
name: authoring-release-docs
description: "Use when producing release-oriented documentation — release notes, changelogs, READMEs, or migration guides. Triggers: 'write release notes', 'generate a changelog', 'create a README', 'document what changed', 'write migration guide', any task involving Jira exports, commit logs, or ticket lists that need to become user-facing documentation. Always load authoring-technical-docs first."
---

# Authoring Release Docs

Produces release notes, changelogs, and READMEs — time-sensitive, high-visibility documentation that users and developers rely on to understand what changed and how to respond.

**Prerequisite:** Load `authoring-technical-docs` first for the multi-pass workflow, style rules, and quality framework. This skill provides release-specific templates and rules only.

---

## Templates

Follow the asset at `./assets/release_notes_template.md` for release notes structure. For changelogs, follow [Keep a Changelog](https://keepachangelog.com) conventions unless a project-specific format is already established.

---

## Core rules

**Framing**
- Lead with user impact, not implementation detail. Write "You can now export dashboards as PDF" — not "Added PDF rendering pipeline."
- Use past tense for all changes: "Added", "Fixed", "Removed", "Deprecated."
- Write for two audiences simultaneously: technical users who need precision, and non-technical stakeholders who need plain-language summaries.

**Structure**
- Open with a one-paragraph summary of the most significant changes in this release.
- Group changes by type in this order: Breaking Changes → New Features → Improvements → Bug Fixes → Deprecations.
- Within each section, order by impact: most significant change first.

**Breaking changes**
- Breaking changes must appear in a prominent callout block — they cannot be buried or easy to miss.
- Always include exact migration steps inline, not just a link to external docs.
- Every deprecation must include a target removal date.

**Bug fixes**
- Be specific. Write "Fixed an issue where CSV exports truncated rows with more than 50 columns" — not "Fixed export bug."
- Include severity where relevant (e.g., data loss, crash, cosmetic).

**Traceability**
- Every bug fix and feature entry must reference its issue tracker ID (e.g., Jira ticket, GitHub issue).
- Every release must include a release date.

**READMEs**
- Must work standalone: a developer finding the repo for the first time should be able to install and run the project using only the README, without consulting any other document.
- The quick-start section must be completable in under 30 seconds.
- Include: project description, prerequisites, installation, quick start, configuration, links to further docs.

---

## Generating release docs from raw inputs

When given Jira exports, commit logs, ticket lists, or PR summaries:

1. **Triage inputs** — identify the type of each item: feature, improvement, bug fix, breaking change, deprecation, or internal/non-user-facing (exclude the last category).
2. **Rewrite for users** — commit messages and ticket titles are written for developers; release notes are written for users. Translate accordingly.
3. **Group and prioritize** — apply the section order above; sort by impact within sections.
4. **Resolve ambiguity conservatively** — if a ticket is too vague to describe accurately, flag it for human review rather than guessing. Do not fabricate specifics.
5. **Verify completeness** — cross-check that every input item is either included or explicitly excluded (with a reason), so nothing is silently dropped.

---

## Output locations

| Document | Path |
|---|---|
| Release notes | `docs/releases/v[version].md` |
| Changelog | `CHANGELOG.md` (project root) |
| README | `README.md` (project root) |
