---
name: write-pr
description: Produce a pull request title and markdown description with a "## Status" testing checklist, plus a screenshots section for UI changes. Use when the student asks to create, write, or draft a PR.
---

# Write PR

Use when the student asks for a PR (title/description).

## Steps

1. Inspect the actual change first (`git diff`, `git log` against the base branch, changed files). Do not invent changes. If the base branch or intent is unclear, ask one question.
2. Decide whether the change touches UI (components, styles, templates, pages). If so, include the screenshot section.
3. Output everything in a single markdown code block so the student can copy it.

## Output format

Title: short, imperative, specific. Follow the repo's commit/PR convention if one exists (e.g. ticket prefix, conventional commits); check `team-conventions` for the team standard.

Description, in this order:

```markdown
## Status
- [ ] <testing item chosen from what this change actually needs>
- [ ] ...

## Summary
<1-3 sentences: what changed and why>

## Changes
- <key change>

## Screenshots   <!-- ONLY for UI changes -->
### Before
<!-- student pastes screenshot -->
### After
<!-- student pastes screenshot -->
```

## Rules

- `## Status` must be the FIRST heading of the description, followed by unchecked `- [ ]` boxes.
- Checklist items are tailored to the change, not generic. Pick from: unit tests added/updated, integration tests, manual test of the specific flow, edge cases/error paths, lint/type-check/build passing, DB migration run, API tested (e.g. Postman), responsive/browser check and accessibility (UI), regression of related features.
- Leave every box unchecked; the student ticks what they actually did. Never claim tests passed unless you ran them and saw output.
- Omit the `## Screenshots` section entirely for non-UI changes.
- Keep it concise; no filler.
