---
name: save-to-vault
description: Save, update, link, or remove explained concept notes in the student's Obsidian vault at C:\Users\alexvek\Documents\Obsidian\Office. Use after explaining an important concept, or when the student asks to review, organize, or recall saved notes.
---

# Save To Vault

Vault root: `C:\Users\alexvek\Documents\Obsidian\Office` (`rootVault`).

## Scope

- Read and search anywhere inside `rootVault` freely.
- Never write outside `rootVault` for vault work.
- Every create, edit, rename, move, or delete needs the student's explicit approval first.
- The note structure below applies to content notes and indexes. Agent instructions, `SKILL.md` files, plugin files, Canvas documents, and Bases have their own formats; do not convert them.

## Approval Protocol

Before any change, show in chat:

1. The action (create / update / rename / delete) and the exact file path.
2. For create or update: the full proposed content, or the exact diff for updates.
3. Any links that will be added to other notes.

Then ask "Save this?" and wait. One approval per listed change; if the plan changes, ask again. Deleting a note also requires listing every note that links to it.

## Before Writing

- Search `rootVault` for an existing note on the concept. Prefer updating it over creating a duplicate.
- Read the target note before proposing edits. Use targeted searches, not a full-vault reread.
- Preserve existing properties, headings, block IDs, embeds, the note's language, and task completion state.
- For Obsidian-specific syntax (aliases, heading/block links, embeds, callouts), read [Obsidian Reference](references/obsidian.md).

## Placement

- A brand-new note (no existing match anywhere in `rootVault`) goes directly in `rootVault/TO_ORGANIZE/`, flat. Use `related: []`. Skip topic-folder search, cross-links, and `_Index.md` updates at creation time.
- Updating an existing note happens in its real location, never in `TO_ORGANIZE`. When updating, link only to notes that really relate (see Linking).
- Topic placement, cross-links, and `_Index.md` updates happen only when the student explicitly asks to organize the vault. Then load `organize-vault`.
- Layout when organizing: `_Index.md` at the root, one `<Topic>/` folder per broad topic (created only when a note belongs in it), one concept per note. Keep `_Index.md` updated in the same approved change set.

## Note Structure

File name: the concept in Title Case, e.g. `Git Rebase.md`. No dates or IDs.

```markdown
---
related:
  - "[[Other Concept]]"
tags: [concept, <topic>]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# <Concept Name>

## <Descriptive Section Heading>
<Content organized to suit this note.>
```

- Properties come first, in the order shown, followed by one `# <Concept Name>` heading matching the file name.
- `related` is a YAML list of quoted wikilinks; use `related: []` when empty. Never save the example placeholder as a real link.
- Keep `created` when updating; set `updated` to the edit date. If an existing note has no creation date, explain the basis of any proposed one (filesystem time is only an estimate).
- Keep extra existing properties (`aliases`, `cssclasses`, `status`, plugin metadata) after the four standard ones, with values and types unchanged.
- Legacy `source` property: move its useful content into a body section instead of discarding it.
- Choose tags that fit the note (an index or task does not need `concept`). Use YAML lists for `related`, `tags`, `aliases`.
- Section names, number, and order depend on the content. Do not force a fixed explanation/examples/mistakes/review template.

## Linking

- Use `[[Concept Name]]` matching the file name; on name collisions use the vault-relative path (`[[Architecture/Feature Flags]]`, forward slashes). Use Markdown links for external URLs.
- List each linked note once in `related`, resolved to its canonical note (heading/block links and embeds count as the containing note). Keep `related` in sync with real body references; do not add self-links, same-note links, attachments, or URLs, and do not drop a meaningful existing relationship just because it has no inline mention.
- Add the reciprocal link in the other note's `related` in the same approved change.
- Never link to a note that does not exist; propose creating it or leave it out.

## Moves, Renames, and Verification

For moves, renames, heading changes, and the post-write checklist, read [Maintenance](references/maintenance.md). After any approved write, run its checks and report only the checks actually performed.

## Writing Style

Write for a human reading months later and for an AI searching it.

- Plain language, descriptive headings, explicit concept names, short paragraphs.
- Bullets for lists, numbered steps for procedures, tables for comparisons, fenced blocks for code. Heading hierarchy: `#` title, `##` sections, `###` subsections.
- Add a Mermaid diagram (simplest fitting type) only when structure is hard to explain in text. Show it in the proposed content, and claim it renders only after the student confirms in Obsidian.
- Optional short callout for a key takeaway. Avoid walls of text, filler, and decoration.
