---
name: organize-vault
description: Drains the Obsidian vault's TO_ORGANIZE inbox into properly structured notes and sweeps the whole vault for broken links, and reports Excalidraw drawings that no note links to.
---

# Organize Vault

Used by the `mentor` agent when the student asks to organize the vault. It processes temporary inbox notes and cleans up dangling links, applying the `save-to-vault` note structure and approval protocol throughout. Load `save-to-vault` alongside this skill before acting.

## Scope

- Inbox folder: `C:\Users\alexvek\Documents\Obsidian\Office\TO_ORGANIZE` (referred to as `inbox`).
- Goal: every note placed in `inbox` ends up merged into, or becomes, a properly structured note elsewhere in the vault, and `inbox` ends up empty.
- Also: every broken wikilink/Markdown link in the vault, and every broken link entry in `_Index.md`, gets found and removed.
- This skill never invents content. It only relocates, merges, and restructures what the student already wrote, plus removes references to notes that no longer exist.

## Part 1 — Draining the inbox

For each file currently in `inbox`:

1. Read the note fully. A single inbox note may contain several unrelated ideas — treat each distinct idea as its own unit of content to place, not the whole file as one unit.
2. For each unit of content, follow `save-to-vault`'s "Before Writing" steps: search the vault for an existing note on that concept.
   - If a matching note exists, propose where in that note the content belongs (new section, extended section, or merged into existing text) rather than appending it unstructured.
   - If no matching note exists, propose a new note in the appropriate topic folder, following `save-to-vault`'s Note Structure and Folder Layout.
3. Follow `save-to-vault`'s Approval Protocol exactly, per note: show the action, the full proposed content or diff, and any links added elsewhere, then ask "Save this?" and wait. One approval per destination note, same as the parent skill.
4. Update `_Index.md` in the same approved change set when a new note is created or a note's topic changes.
5. Only after every unit of content from the inbox file has been placed and approved, propose deleting the original inbox file. State the file path and confirm nothing from it was left unplaced. Ask "Delete the original from TO_ORGANIZE now?" separately from the save approvals above — this is a distinct, destructive step and needs its own yes.
6. On approval, delete it with:
   ```
   Remove-Item -Path "C:\Users\alexvek\Documents\Obsidian\Office\TO_ORGANIZE\<filename>"
   ```
   This exact command form is the only delete permission granted for this work (see the `Remove-Item *TO_ORGANIZE*` allow rule in `agents/mentor.md`). Do not attempt `rm`, `del`, or a path outside `TO_ORGANIZE` — those are not permitted and will be blocked or asked for separately. If either this command or that permission pattern changes, update the other in the same edit, or deletion will start asking for approval or failing silently.

If an inbox note is empty, a duplicate of existing content with nothing new, or unintelligible, say so and ask the student how to handle it instead of guessing.

## Part 2 — Vault-wide broken link sweep

1. Search every note in the vault for wikilinks (`[[Target]]`, `[[Target#Heading]]`, `[[Target|Label]]`) and Markdown links to local `.md` files.
2. For each link, resolve the target to a real file path and check it exists. Skip external URLs, attachment embeds (images, PDFs), and same-note heading/block links (`[[#Heading]]`) — those aren't vault-note links.
3. Build one list of every broken link found, grouped by the file it appears in, showing the surrounding text so the student can see context.
4. Present the full list once and ask a single approval: "Remove these links?" Do not ask per file.
5. On approval, edit each source file: remove the broken link. If the link is the entire content of its line/list item (common in indexes, bullet lists of "See also" references), remove the whole line rather than leaving an empty bullet. If the link is inline within a sentence, remove just the link markup and judge whether the surrounding sentence still reads correctly — flag any sentence that would become broken or confusing instead of silently mangling it.

## Part 3 — `_Index.md` specifically

`_Index.md` entries are typically `- [[Note Name]] — description` or similar. Treat it as covered by Part 2's scan, but double check explicitly: for every link in `_Index.md`, verify the target note exists. If not, remove that entire index line (link + description together), not just the link syntax, since a dangling description with no link is equally useless. Include these in the same batch list and approval as Part 2 — don't ask twice.

## Part 4 — Orphan Excalidraw drawings (always run)

Always check the `Excalidraw` folder, every run, even when the inbox is empty and no broken links were found.

1. List every drawing file in `C:\Users\alexvek\Documents\Obsidian\Office\Excalidraw` (`*.excalidraw.md`). Use `read` on the folder if `glob` fails.
2. For each drawing, check whether any note in the vault links to or embeds it, e.g. `[[Name]]`, `![[Name]]`, `[[Name.excalidraw]]`, or `[[Name|Label]]`. Search all notes outside `Excalidraw`; if `grep` fails, read the notes directly. A drawing's own file does not count as a link.
3. Also scan each drawing's readable "Text Elements" section for broken note links, and say that the compressed drawing data cannot be checked.
4. Report a list of orphan drawings (drawings no note links to) and state which notes were checked. If there are none, say so explicitly.
5. Report only. Do not delete, move, or link drawings without a separate approval from the student. Offer to add an embed in a suitable note, following the `save-to-vault` approval protocol.

## After Writing

Run `save-to-vault`'s own verification checklist (valid YAML, resolving links, balanced fences, index agreement) on every note touched in Part 1, and re-check that no new broken links were introduced in Part 2/3 edits themselves.

## Version Control

Do not run `git add`, `git commit`, or `git push` yourself. After the run, suggest one commit message covering all changes made (inbox drains, deletions, and link cleanup can be one commit if done in one session) and give the copy-pasteable block from `AGENTS.md` for the student to run.
