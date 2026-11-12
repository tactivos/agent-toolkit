---
name: templates
description: Add a Mural template onto the current mural and optionally fill it in with content. Use whenever a user asks to add, insert, apply, or start from a template — a retro, icebreaker, brainstorm, workshop canvas, LUMA method, or any named Mural template — instead of rebuilding the layout widget by widget with create_* tools.
---

# Working with Mural templates

Mural ships a curated library of templates (including partner methods such
as LUMA). The `add_template_to_canvas` tool drops one onto the
current mural with full fidelity — the Mural app itself inserts the widgets,
so photos, clusters, tables, and connectors all come through, as one undoable
action.

**Never rebuild a known template by hand** with `create_stickies` /
`create_shapes` / etc. A rebuild loses images, clusters, and styling, and
takes dozens of calls; the template insert is one call.

## The flow

1. **Find the template id.** If a template search tool is available in your
   toolset, use it and prefer its results. Otherwise ask the user for the
   template (they can copy the id from the template's URL in Mural's template
   library). Only templates from Mural's built-in library work —
   workspace/custom template ids fail with an error.
2. **Add it:** `add_template_to_canvas(template_id=...)`. By default it lands
   to the right of existing content (or centered at the origin on an empty
   canvas). Pass both `x` and `y` to place the template's top-left corner
   explicitly — useful when the user points at a spot ("below the retro
   area").
3. **Fill it in (on request).** The response returns `widget_ids` for every
   created widget. "Add the template and fill it in" is a normal follow-up:
   read the inserted structure (`list_widgets` or `get_widget_by_id` on the
   returned ids, or `get_canvas_overview`), then use the regular edit tools
   (`update_widgets`, `create_stickies` into the template's areas, …) to
   populate it with the user's content.

## Behavior notes

- A mural is an infinite canvas: templates are **added to** the mural
  alongside existing content. Nothing is replaced. There is no
  "create a mural from a template" — compose `create_mural` +
  `add_template_to_canvas` if the user wants a fresh mural: select the
  new mural id and Canvas Host loads it on the first canvas operation.
- Inserted widgets are attributed as AI-assisted ("with AI") automatically.
- Template comments are intentionally dropped on insert (matches the in-app
  behavior).
- Failures are real: a failed insert never reports success. An empty
  template can legitimately succeed with `widgets_added: 0`, so check the
  count rather than assuming success means widgets landed.
  A "method not found" error names its fix in the error message; follow it.
