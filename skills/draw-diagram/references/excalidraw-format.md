# Excalidraw Format Reference

Used by the `draw-diagram` skill. This file has two layers:

1. **Technical Contract** (fixed). Follow it exactly, or the drawing is invalid or broken.
2. **Design Palette** (a menu). Choose from it by understanding the topic. Nothing in this layer is mandatory, and a diagram should look like what it explains, not like the previous diagram.

The technical contract was verified by opening a drawing in the Excalidraw plugin in Obsidian. Items marked **(unverified)** come from the standard Excalidraw format but have not yet been rendered in this vault's plugin. The first drawing that uses one must be checked by the student, and then the mark can be removed here.

---

# Layer 1: Technical Contract

## File Wrapper

A drawing is one Markdown file named `<Note Name> - <What It Shows>.excalidraw.md` with this exact shape:

````markdown
---
excalidraw-plugin: parsed
tags: [excalidraw]
---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# Excalidraw Data

## Text Elements
<one line per text element: `Visible text ^elementId`, blank line between entries>

## Drawing
```json
{ "type": "excalidraw", "version": 2, "source": "https://excalidraw.com",
  "elements": [ ... ],
  "appState": {"viewBackgroundColor": "#ffffff"},
  "files": {} }
```
%%
````

Rules:

- The closing `%%` line is required, directly after the JSON fence.
- `## Text Elements` must list every element of type `text`, as `Text ^id`, where `id` equals the element's `id` in the JSON. Keep both in sync.
- Do not add other properties to the frontmatter, and do not add a `# Title` heading or concept-note structure. The plugin owns this file's format.

## Identity and Binding Rules

- Every `id` is unique. Every `seed` and `versionNonce` is unique (using the same number for both is fine).
- A text inside a shape has `containerId` set to the shape's `id`, and the shape lists that text in its `boundElements` as `{"id":"<textId>","type":"text"}`.
- An arrow's `startBinding.elementId` and `endBinding.elementId` are shape ids. Every shape an arrow touches lists that arrow in its `boundElements` as `{"id":"<arrowId>","type":"arrow"}`.
- Free labels (titles, frame labels, arrow labels) have `containerId: null` and are not bound to anything.
- Draw order is array order. Put frames first, then shapes and their text, then arrows, then free labels.

## Arrow Geometry

- `x`, `y` is the arrow's start point, normally the middle of the facing edge of the source shape.
- `points` is `[[0,0],[DX,DY]]`, the offset from the start point to the end point (the middle of the facing edge of the target).
- `width` and `height` are the absolute values of `DX` and `DY`. A leftward or upward arrow has a negative `DX` or `DY` in `points` only.
- Examples (shape `x,y,w,h`):
  - Neighbour to the right: start `(x + w, y + h/2)`, `points` `[[0,0],[gap,0]]`.
  - Neighbour below: start `(x + w/2, y + h)`, `points` `[[0,0],[0,gap]]`.
  - Diagonal: start at the facing edge middle, `points` `[[0,0],[DX,DY]]` with both non-zero.
- Two arrows between the same pair of shapes (one each way, or two relationships): offset them by `35` px, one above and one below the center line, so they never overlap.

## Element Templates

Replace the `<PLACEHOLDERS>`. All templates share these base fields: `angle:0, groupIds:[], version:1, isDeleted:false, updated:1, link:null, locked:false, opacity:100`.

### Shape (rectangle, ellipse or diamond)

```json
{"id":"<ID>","type":"<rectangle|ellipse|diamond>","x":<X>,"y":<Y>,"width":<W>,"height":<H>,"strokeColor":"<STROKE>","backgroundColor":"<FILL>","fillStyle":"solid","strokeWidth":2,"roughness":1,"opacity":100,"angle":0,"groupIds":[],"seed":<N>,"version":1,"versionNonce":<N>,"isDeleted":false,"boundElements":[{"id":"<TEXT ID>","type":"text"},{"id":"<ARROW ID>","type":"arrow"}],"updated":1,"link":null,"locked":false}
```

Optional fields to vary (see Design Palette): `strokeStyle` (`"solid"` default, `"dashed"`, `"dotted"`), `fillStyle` (`"solid"`, `"hachure"`, `"cross-hatch"`), `roughness` (`0`, `1`, `2`), `strokeWidth`, `roundness` (`{"type":3}` for rounded corners, rectangles only) **(unverified)**.

### Shape text (text inside a shape)

```json
{"id":"<TEXT ID>","type":"text","x":<CENTERED X>,"y":<CENTERED Y>,"width":<TEXT WIDTH>,"height":25,"text":"<TEXT>","fontSize":20,"fontFamily":1,"textAlign":"center","verticalAlign":"middle","containerId":"<SHAPE ID>","originalText":"<TEXT>","strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"roughness":1,"opacity":100,"angle":0,"groupIds":[],"seed":<N>,"version":1,"versionNonce":<N>,"isDeleted":false,"boundElements":null,"updated":1,"link":null,"locked":false}
```

- `text` equals `originalText`. Keep shape labels to one line.
- Text width estimate: about `0.5 x fontSize` per character, rounded up (fontSize 20 gives about 10 px per character).
- Centered: `x = shape.x + (shape.width - text.width) / 2`, `y = shape.y + (shape.height - text.height) / 2`.

### Arrow

```json
{"id":"<ARROW ID>","type":"arrow","x":<START X>,"y":<START Y>,"width":<ABS DX>,"height":<ABS DY>,"points":[[0,0],[<DX>,<DY>]],"startBinding":{"elementId":"<FROM>","focus":0,"gap":1},"endBinding":{"elementId":"<TO>","focus":0,"gap":1},"startArrowhead":null,"endArrowhead":"arrow","strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"roughness":1,"opacity":100,"angle":0,"groupIds":[],"seed":<N>,"version":1,"versionNonce":<N>,"isDeleted":false,"boundElements":null,"updated":1,"link":null,"locked":false}
```

Optional: `strokeStyle` `"dashed"`, `startArrowhead` set for a two-way arrow, arrowhead values `"arrow"`, `"triangle"`, `"bar"`, `"dot"` **(unverified except `"arrow"`)**.

### Frame (a rectangle that groups other shapes)

```json
{"id":"<ID>","type":"rectangle","x":<X>,"y":<Y>,"width":<W>,"height":<H>,"strokeColor":"<STROKE>","backgroundColor":"transparent","fillStyle":"solid","strokeStyle":"dashed","strokeWidth":2,"roughness":0,"opacity":100,"angle":0,"groupIds":[],"seed":<N>,"version":1,"versionNonce":<N>,"isDeleted":false,"boundElements":null,"updated":1,"link":null,"locked":false}
```

Leave `40` px of padding between the frame edge and the shapes inside, and `60` px above the first shape for the frame label.

### Free label (title, frame label, arrow label, legend entry)

```json
{"id":"<ID>","type":"text","x":<X>,"y":<Y>,"width":<TEXT WIDTH>,"height":<H>,"text":"<TEXT>","fontSize":<16|22|32>,"fontFamily":1,"textAlign":"<left|center>","verticalAlign":"top","containerId":null,"originalText":"<TEXT>","strokeColor":"<COLOR>","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":1,"roughness":1,"opacity":100,"angle":0,"groupIds":[],"seed":<N>,"version":1,"versionNonce":<N>,"isDeleted":false,"boundElements":null,"updated":1,"link":null,"locked":false}
```

Heights by fontSize: `16` gives `20`, `22` gives `28`, `32` gives `40`. Title: fontSize `32` above everything. Frame label: fontSize `22`, at the top of its frame. Arrow label: fontSize `16`, about `30` px above a horizontal arrow or `10` px to the right of a vertical arrow, away from other lines.

## Self-check Before Proposing a Drawing

1. Every `id` appears once. Every `containerId`, `startBinding.elementId` and `endBinding.elementId` points to an existing id.
2. Every shape lists its text and every arrow that touches it, and every arrow's two ends are listed in those shapes.
3. `## Text Elements` lists every text element with the right `^id`.
4. The JSON has balanced braces and brackets, and no trailing commas.
5. No two shapes overlap, no arrow crosses a shape it is not connected to, and no label sits on a shape or another label.
6. Text fits inside its shape (shape width is at least text width + 40).
7. The file ends with the closing fence and `%%`.

---

# Layer 2: Design Palette

## How to Choose

Before drawing, decide these four things from the content, and say them in one line each when proposing the drawing:

1. **What the reader should understand in 10 seconds** (the one idea: a sequence, a hierarchy, a loop, a comparison, a layered system).
2. **Diagram type and layout** (below).
3. **Shapes** (below).
4. **Palette**: what each color means in this drawing.

Prefer the simplest diagram that carries the idea. Fewer nodes beat more nodes: about 15 shapes at most, and if more are needed, split into two drawings with different suffixes.

## Layouts by Topic Type

| The content is mostly... | Layout | Geometry guide |
|---|---|---|
| Steps in order, request/response | Left-to-right flow, wrapping to a new row after about 5 shapes | Horizontal gap 100 to 120, vertical gap 60 |
| A parent/child structure, taxonomy | Top-down tree | Parent centered above its children, vertical gap 80, sibling gap 40, arrows from parent bottom to child top |
| A repeating process, feedback loop | Ring of 4 to 6 shapes, arrows clockwise | Place shape centers on a circle of radius about 220, arrows between facing edges |
| Two or more options compared | Side-by-side columns, one frame each | Matching items on the same rows, no arrows between columns, or dashed ones for "maps to" |
| A system with tiers or ownership | Stacked bands (one wide frame per tier) | Arrows go vertically between bands, shapes aligned in columns |
| Events over time | Single row, ordered by time | Dates or phases as free labels under each shape, arrows between neighbours |
| A decision | Flow with a diamond, one arrow out per outcome | Label each outcome on its arrow ("yes" / "no") |

A topic can mix layouts, for example a flow inside a tiered system. Keep one dominant direction.

## Shapes

| Use | For |
|---|---|
| Rectangle | A component, service, step, or thing (default) |
| Rectangle with `roundness` | A softer look for processes or user-facing items **(unverified)** |
| Ellipse | A start/end point, an actor, or an external system. Make it about 1.4 x wider than the rectangle for the same text, because text has less room. **(unverified)** |
| Diamond | A decision or condition. Make it about 1.8 x wider and 1.6 x taller than a rectangle for the same text. **(unverified)** |
| Frame (dashed) | A group, phase, boundary, or tier |

Size shapes from their text: width at least `text width + 40`, minimum `140`, height `70` for one line of text. Use the same size for shapes of the same role.

## Line and Fill Styling

- **Arrows:** solid for the main or synchronous path, dashed for optional, asynchronous or "planned" links. Use a different color for a path that goes against the main flow (a push, a callback, an error path).
- **Frames:** dashed. Use a neutral grey for frames that group *phases or topics*, and a role color when the frame stands for a real tier (for example a frontend tier).
- **Fill:** `solid` is the default. Use `hachure` for something planned, optional or unfinished, and say so in a legend.
- **Roughness:** `0` (clean, architect style) for technical drawings, such as architecture and protocols. `1` (sketchy) for concept or explanatory drawings. Use one value per drawing, except frames, which stay at `0`.
- **Emphasis:** use `strokeWidth` `3` on one or two shapes that are the key idea. Do not emphasize more than that.

## Color

Choose colors by what they mean **in this drawing**. Do not reuse a fixed mapping from another drawing.

Guardrails:

- Use at most 4 or 5 fill colors, and the same color always means the same thing within one drawing.
- Light fills with dark text (`#1e1e1e`). Never put light text on a light fill.
- Use one stroke color per drawing for shapes (usually `#1e1e1e`), and color only the fill.
- When colors carry meaning, add a legend: a small shape of each color with a free label next to it, placed in a corner outside the main layout.
- If the color adds nothing (everything is the same kind of thing), use one light fill or none. A plain drawing is fine.

Palette to draw from (fill, matching stroke for frames or emphasis):

| Fill | Stroke | Common meaning candidates |
|---|---|---|
| `#a5d8ff` | `#1971c2` | user side, input, client, current state |
| `#b2f2bb` | `#2f9e44` | success, service, internal logic, done |
| `#ffc9c9` | `#e03131` | risk, error, cache, danger, blocker |
| `#d0bfff` | `#7048e8` | storage, data, persistent state |
| `#ffec99` | `#f08c00` | human actor, warning, decision |
| `#ffd8a8` | `#e8590c` | events, async, messaging, external system |
| `#99e9f2` | `#0c8599` | network, infrastructure |
| `#fcc2d7` | `#c2255c` | highlight, "important" |
| `#dee2e6` | `#868e96` | neutral, grouping, background, disabled |

The mapping column is a suggestion. Another assignment is fine when it fits the topic better.

## Legend (when colors carry meaning)

Place in the bottom-right corner, outside the main layout: for each color used, a `40 x 24` rectangle and a fontSize `16` free label to its right, stacked with 10 px between rows. Legend shapes are not bound to anything.

## Known Limits

- Verified in this vault: rectangles, text bound to rectangles, straight arrows with bindings, dashed frames, free labels.
- **(unverified)**: ellipse, diamond, `roundness`, `hachure`/`cross-hatch` fills, arrowheads other than `"arrow"`, dotted lines, bound text inside ellipses and diamonds. After the first drawing using one of these renders correctly, remove its mark here.
- Arrows are straight lines. Diagonal arrows can cross labels, so check them in the rendered view.
- A new drawing is not verified until the student opens it and confirms it renders.
