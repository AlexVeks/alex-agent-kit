---
name: save-to-vault
description: Save, update, link, or remove explained concept notes in the student's Obsidian vault at C:\Users\alexvek\Documents\Obsidian\Office. Use after explaining an important concept, or when the student asks to review, organize, or recall saved notes.
---

# Save To Vault

Vault root: `C:\Users\alexvek\Documents\Obsidian\Office` (referred to as `rootVault`).


## Scope

- Read and search anywhere inside `rootVault` freely.
- Never write outside `rootVault` for vault work, even elsewhere in the Obsidian vault.
- Every create, edit, rename, move, or delete needs the student's explicit approval first.
- Apply the note structure below to content notes and indexes. Agent instructions, `SKILL.md` files, plugin files, Canvas documents, and Bases have their own formats; do not convert them into concept notes during vault organization.

## Approval Protocol

Before any change, show in chat:

1. The action (create / update / rename / delete) and the exact file path.
2. For create or update: the full proposed note content, or the exact diff for updates.
3. Any links that will be added to other notes.

Then ask "Save this?" and wait. Only one approval per listed change; if the plan changes, ask again. Deleting a note also requires listing every note that links to it, so links are not left broken.

## Before Writing

- Search `rootVault` for an existing note on the concept. Prefer updating it over creating a duplicate.
- Find related notes to link to. Link only when the relationship is real.
- Read the target note and the relevant part of `rootVault/_Index.md` before proposing edits. Use targeted searches rather than rereading the entire vault for a small update.
- Inspect existing properties, headings, block IDs, and embeds before restructuring. Preserve references and useful content, including the note's language and task completion state.
- For Obsidian-specific syntax, read [Obsidian Reference](references/obsidian.md). It covers aliases, heading/block links, embeds, callouts, and optional app-aware inspection.

## Folder Layout

```text
rootVault/
  _Index.md              Map of all notes, grouped by topic
  <Topic>/               One folder per broad topic (e.g. Git, Python, Testing)
    <Concept Name>.md    One concept per note
```

Create a topic folder only when at least one note belongs in it. Keep `_Index.md` updated in the same approved change set as the note.

## Note Structure

File name: the concept in Title Case, e.g. `Git Rebase.md`. No dates or IDs in the name.

Only the YAML properties and the main heading have a fixed structure. Put the properties at the very top, followed by one `# <Concept Name>` heading matching the file name. Use the property order shown below.

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
<Content organized to suit this particular note.>

## <Another Relevant Section Heading>
<Further explanation, examples, steps, or reference material as needed.>
```

- `related` replaces the `source` property. It is a YAML list of quoted Obsidian wikilinks to the existing notes this note links to. Use `related: []` when there are no linked notes; never save the example placeholder as a real link.
- Keep related links at the top in the properties, not in a required `Related` section at the bottom. Inline links can also appear where they help explain a connection.
- Preserve the original `created` date when updating a note; set `updated` to the date of the edit.
- If an existing note has no recorded creation date, explain the basis of any proposed date. Filesystem creation time is an estimate of note age, not proof of its authoring date.
- When migrating an existing note, preserve useful information from `source` in a suitable body section rather than discarding it.
- Keep existing additional properties, such as `aliases`, `cssclasses`, `status`, or plugin metadata, after the four standard properties. Preserve their values and YAML types unless an approved change specifically modifies them. The approved migration of `source` into the body is the exception.
- Choose tags that match the note's purpose; an index or task does not need the `concept` tag. Use YAML lists for `related`, `tags`, and `aliases`.
- The body must be divided into clearly named sections, but their names, number, and order depend on the content. The example headings above are placeholders, not required headings.
- Do not force every note into the same explanation, motivation, examples, mistakes, or review-question template. Include those sections only when they help that particular note.

## Linking Rules

- Use `[[Concept Name]]` matching the file name without `.md`. When names collide, use the vault-relative path, such as `[[Architecture/Feature Flags]]`, to identify the target unambiguously. Use forward slashes in vault links.
- Use wikilinks for vault notes and Markdown links for external URLs. Display labels, headings, and block references are supported; verify the target rather than treating the visible label as a filename.
- Store each distinct linked note once in the top-level `related` property, with each wikilink quoted for valid YAML. Resolve links to their actual note before deduplicating; represent heading/block links and Markdown-note embeds with the containing note's canonical link.
- Keep `related` in sync with actual note references in the body. Same-note navigation, attachment links/embeds, external URLs, and illustrative link syntax in code or comments do not belong in it. Do not add self-links or remove a meaningful existing relationship merely because it has no inline mention.
- Add the backlink in the other note too, as part of the same approved change.
- Put reciprocal links in the other note's top-level `related` property as well. Explain the relationship naturally in the body when it would not otherwise be clear.
- Never link to a note that does not exist; propose creating it or leave it out.

## Moves, Renames, and Heading Changes

- Find incoming references before a move or rename, including qualified wikilinks, Markdown links, embeds, and relevant Canvas or Bases file references. Include required reference updates and the index in the same proposed change set.
- Check references to headings and block IDs before changing them. Preserve stable IDs and anchor text where possible; otherwise include dependent link updates in the proposal.
- Preserve existing attachment targets and embed options such as image widths and PDF page selectors. Report pre-existing missing targets separately from any regressions caused by the edit.
- Link-aware Obsidian operations can help with refactors when the app and CLI are available. They still require the same approved targets and edits; verify the actual affected references afterward.

## Tools and Verification

- Prefer the available read, search, and patch tools for ordinary filesystem work. Use Obsidian's CLI only when the task benefits from its live index or configured behavior, such as resolved backlinks, typed properties, or open tabs. Follow the capability probe and exact targeting rules in [Obsidian Reference](references/obsidian.md); CLI availability is optional.
- After an approved change, check the affected notes for valid YAML, quoted link values, the four standard properties, preserved extra metadata, a single matching title, a sensible heading hierarchy, and balanced code fences. Account for fenced examples when inspecting headings or links.
- Verify that note targets resolve unambiguously, referenced headings/block IDs exist, `related` entries are distinct, and reciprocal links and index entries agree. Validate attachment targets separately from note links.
- After moves or renames, check the destination, the absence of unintended duplicate copies, and references to the old paths. For large formatting-only changes, compare the retained content to the original.
- Report the checks actually performed and any unresolved targets. Distinguish a text/structure check from YAML parsing or Obsidian rendering; do not claim a parser or runtime check that was not run.

## Writing Style

Write for a human reading it months later and for an AI searching it:

- Use plain language, descriptive section headings, and explicit concept names so the note is easy to skim and retrieve.
- Keep paragraphs short and separate sections with whitespace. Use bullets for lists, numbered steps for procedures, tables for useful comparisons, and fenced code blocks for code.
- Use a clear heading hierarchy (`#` for the title, `##` for sections, `###` for subsections); consistency means the same hierarchy, not identical section names across notes.
- Choose formatting that suits the information. Avoid walls of text, unnecessary sections, decorative clutter, and chat filler.
- Use a short, optional callout for a useful takeaway or a collapsible detail when it improves readability. For long notes, consider a short set of same-note heading links for navigation rather than repeating the content.
