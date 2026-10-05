---
name: sketch-whiteboard
description: Draw a diagram (architecture, flow, mind map, timeline) on a new static.md Excalidraw whiteboard and give the user its link. Use when the user asks to sketch, draw or diagram something they can open, share and keep editing by hand.
---

# Sketch a static.md whiteboard

Create an Excalidraw whiteboard with `create_board`, seeded with the diagram, and hand back its link.

## Building the scene

Pass `excalidraw_json` as a JSON string: either `{"elements": [...]}` or a bare array of elements. Every element needs a unique string `id` and a `type`. static.md loads the scene through Excalidraw, which fills in any styling fields you leave out.

Lay the diagram out on a grid so nothing overlaps: boxes about 200×80, 120 px apart horizontally and 160 px vertically.

A labelled box and an arrow to the next box:

```json
{"elements": [
  {"id": "api", "type": "rectangle", "x": 0, "y": 0, "width": 200, "height": 80,
   "strokeColor": "#1e1e1e", "backgroundColor": "#a5d8ff", "fillStyle": "solid", "roundness": {"type": 3},
   "boundElements": [{"id": "api-label", "type": "text"}]},
  {"id": "api-label", "type": "text", "x": 60, "y": 27, "width": 80, "height": 25,
   "text": "API", "originalText": "API", "fontSize": 20, "fontFamily": 5,
   "textAlign": "center", "verticalAlign": "middle", "containerId": "api"},
  {"id": "db", "type": "rectangle", "x": 320, "y": 0, "width": 200, "height": 80,
   "strokeColor": "#1e1e1e", "backgroundColor": "#b2f2bb", "fillStyle": "solid", "roundness": {"type": 3},
   "boundElements": [{"id": "db-label", "type": "text"}]},
  {"id": "db-label", "type": "text", "x": 360, "y": 27, "width": 120, "height": 25,
   "text": "Database", "originalText": "Database", "fontSize": 20, "fontFamily": 5,
   "textAlign": "center", "verticalAlign": "middle", "containerId": "db"},
  {"id": "api-db", "type": "arrow", "x": 200, "y": 40, "width": 120, "height": 0,
   "points": [[0, 0], [120, 0]], "endArrowhead": "arrow", "strokeColor": "#1e1e1e"}
]}
```

- Other shapes: `ellipse` and `diamond` take the same fields as `rectangle`.
- A free-standing label is a `text` element without `containerId`.
- An arrow's `points` are relative to its own `x` and `y`.

## Steps

1. Plan the layout first: which boxes, which arrows, roughly where each one goes.
2. Call `create_board` with a short `title` (static.md keeps the first 32 characters) and the scene.
3. Reply with the link from the result (`https://static.md/draw/<id>`). New whiteboards open to anyone with the link, and anyone who has it can edit it, so say so in one short line.
4. Change who can open it only when the user asks, with `set_access` and `kind: "board"`. Restricting access is a Pro feature.

## Limits

- Free plan whiteboards hold up to 300 elements; Pro removes the limit. One call can seed about 500 KB of scene.
- On the free plan, agents can create 10 notes or whiteboards per hour and make 50 tool calls a day. If a call is refused, tell the user what the result says instead of retrying.
- To look at an existing whiteboard, find it with `list_my_boards` and open it with `read_board`. Agents can't edit an existing whiteboard; create a new one instead.
