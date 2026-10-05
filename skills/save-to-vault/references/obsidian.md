# Obsidian Reference

Read the sections relevant to the current task. The parent skill's vault scope, note structure, and approval protocol govern every example below.

## Note Links and Navigation

| Purpose | Syntax |
|---|---|
| Link to a note | `[[Feature Flags]]` |
| Disambiguate a filename | `[[Architecture/Feature Flags]]` |
| Show a different label | `[[Feature Flags\|Runtime toggles]]` |
| Link to a section | `[[Feature Flags#Evaluating a Flag]]` |
| Navigate inside this note | `[[#Evaluating a Flag]]` |
| Link to a block | `[[Feature Flags#^flag-example]]` |

The backslash before `|` in the table above escapes a Markdown table separator. Outside a table, write the display-label link as `[[Feature Flags|Runtime toggles]]`.

Verify the real target path, heading text, and block ID. A block ID such as `^flag-example` identifies a paragraph or block; for a list or quote, place the ID on its own line after the block.

For `related`, both `[[Feature Flags#Evaluating a Flag]]` and `[[Feature Flags|Runtime toggles]]` resolve to the single entry `"[[Feature Flags]]"`. Preserve path qualification when it is needed to distinguish notes. A same-note heading link adds no entry.

## Properties

Keep YAML frontmatter at the very beginning of the file. Quote wikilinks as YAML strings, and preserve existing property types. Reserved properties such as `aliases` and `cssclasses` can affect Obsidian behavior.

Worked example of the standard header with additional metadata:

```yaml
---
related:
  - "[[CI and CD]]"
tags: [concept, architecture]
created: 2026-10-04
updated: 2026-10-05
aliases:
  - Runtime Toggles
cssclasses:
  - wide-page
status: draft
---
```

This is a syntax example, not a request to add these aliases, CSS classes, status values, or dates to every note. Use `related: []` when there are no related notes. Tags can use nested paths such as `project/bta`; do not prefix frontmatter tag values with `#`.

## Embeds

```markdown
![[Feature Flags]]
![[Feature Flags#Evaluating a Flag]]
![[Feature Flags#^flag-example]]
![[diagram.png|300]]
![[diagram.png|640x480]]
![[reference.pdf#page=3]]
```

Markdown-note embeds contribute the containing note to `related`. Image, audio, video, PDF, Canvas, and Base embeds are asset/view references rather than related Markdown notes. Preserve their paths and options, and validate that the target exists. Never substitute an unrelated attachment to hide a missing file.

## Optional Callouts

```markdown
> [!tip] Key distinction
> Deployment delivers code; a feature flag controls its availability.

> [!example]- Additional detail
> This section starts collapsed and can be expanded by the reader.
```

Use callouts only when the content benefits from emphasis or folding. Normal headings and paragraphs remain the default; important facts should remain easy to find.

## Optional Obsidian CLI

Use app-aware inspection for tasks that need Obsidian's parsed metadata, resolved links, configured features, or live workspace. Normal reading and editing use the existing filesystem tools.

Before relying on the CLI, probe availability in PowerShell:

```powershell
$obsidianCli = if ($env:COPILOT_OBSIDIAN_CLI) { $env:COPILOT_OBSIDIAN_CLI } else { "obsidian" }
& $obsidianCli version
```

The environment variable is optional; it may be supplied by a Copilot-hosted session. A command on PATH is not enough: require a successful probe. If unavailable, use filesystem checks and explain any runtime-only limitation that matters. Do not install software or modify PATH as part of a note edit.

Resolve the executable in each shell call that needs it. Inspect live `help` for the installed version before using a command. Confirm the vault name or ID corresponds to `rootVault`; do not infer the target from whichever vault or note is active. Put `vault=<confirmed-name-or-id>` before the command and use `path=<vault-relative-path>` for exact file targeting. Reserve `file=` for intentional name-based resolution.

Useful command families to discover through live help:

- `links`, `backlinks`, `unresolved`: app-resolved relationships and missing targets.
- `properties`, `property:read`: parsed and typed note metadata.
- `tabs`, `workspace`: currently open notes and layout; recent files are not proof that a note is open.
- `move`, `rename`: app-aware refactors when the exact changes have been approved.

Prefer structured output when the installed command supports it. CLI operations do not broaden vault scope or approval. If the session is hosted inside Obsidian, avoid restarting/reloading the app or the plugin hosting the session; that can terminate the active work.

## Sources Reviewed

These task-specific rules and examples were informed by the vault's `copilot/skills/obsidian-markdown/` and `copilot/skills/obsidian-cli/` documents and the Markdown skill's properties, embeds, and callouts references. Those documents identify `kepano/obsidian-skills` revision `a1dc48e68138490d522c04cbf5822214c6eb1202` as their upstream source.
