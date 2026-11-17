---
name: draw-diagram
description: Propose an Excalidraw diagram file for the student's Obsidian vault when a concept is easier to understand as a picture than as text. Use after or while explaining a flow, hierarchy, loop, comparison, or layered system, or when the student asks for a drawing.
---

# Draw Diagram

Creates one `.excalidraw.md` drawing in the vault's `Excalidraw` folder. The file format is fragile, so read [Excalidraw Format](references/excalidraw-format.md) in full before drafting.

## Scope

- Vault root and approval rules come from `save-to-vault`. Load it alongside this skill. Every create, edit, rename, or delete needs the student's explicit approval first.
- Drawings live in `C:\Users\alexvek\Documents\Obsidian\Office\Excalidraw`.
- File name: `<Note Name> - <What It Shows>.excalidraw.md`.
- A drawing is not complete until a concept note links to or embeds it (`![[Name.excalidraw]]`). An unlinked drawing is reported by `organize-vault` as an orphan.

## Steps

1. Identify the one idea the reader should get in 10 seconds. If the idea is not a sequence, hierarchy, loop, comparison, or layered system, text is probably better; say so and skip the drawing.
2. State in one line each: the layout, the shapes, and what each color means (see the Design Palette in the reference).
3. Draft the drawing following the Technical Contract exactly. Keep it to about 15 shapes; split into two drawings if more are needed.
4. Run the reference's Self-check on the draft before showing it.
5. Show the approval block from `save-to-vault` (path, full content, the note that will link to it), then ask "Save this?" and wait.
6. After saving, tell the student to open the file in Obsidian's Excalidraw view and confirm it renders. Do not claim it renders until they do.

## Rules

- Never invent a drawing for a concept you have not explained first.
- Do not use elements marked **(unverified)** in the reference unless the diagram needs them, and tell the student which ones you used so they can check them.
- Do not convert or restyle drawings the student made by hand.
