# Vault Maintenance and Verification

Load this when moving, renaming, or restructuring notes, and for the post-write verification checklist.

## Moves, Renames, and Heading Changes

- Find incoming references before a move or rename: wikilinks (including qualified ones), Markdown links, embeds, and relevant Canvas or Bases file references. Include required reference updates and the index in the same proposed change set.
- Check references to headings and block IDs before changing them. Keep stable IDs and anchor text where possible; otherwise include dependent link updates in the proposal.
- Preserve attachment targets and embed options (image widths, PDF page selectors). Report pre-existing missing targets separately from regressions caused by the edit.
- Obsidian CLI link-aware operations may help when available. They need the same approved targets and edits; verify the affected references afterward.

## Verification After an Approved Change

Check the affected notes for:

- valid YAML, quoted link values, the four standard properties, preserved extra metadata
- a single matching `#` title, sensible heading hierarchy, balanced code fences (account for fenced examples when inspecting headings or links)
- note targets that resolve unambiguously, referenced headings/block IDs that exist, distinct `related` entries, and reciprocal links and index entries that agree
- attachment targets, validated separately from note links

After moves or renames, also check the destination, that no unintended duplicate copies exist, and that no references to the old paths remain. For large formatting-only changes, compare the retained content to the original.

Report the checks actually performed and any unresolved targets. Distinguish a text/structure check from YAML parsing or Obsidian rendering; do not claim a parser or runtime check that was not run.
