---
name: mural-design
description: Apply Mural's template design principles when creating, laying out, or arranging any content on a Mural board through the Mural MCP — activities, templates, matrices, retros, workshops, diagrams, brainstorms, or any multi-widget layout. Use this whenever a user asks to build, make, set up, populate, generate, or design a mural or an activity on one, even if they never say "design," "template," or "principles." It covers dimensions and spacing (sticky sizes, gaps, area sizing), layout and flow, color semantics, typography, iconography, and the correct MCP tool choices (areas vs. tables, lines vs. connectors, create_stickies spacing behavior) so boards come out looking like Mural's own templates rather than generic whiteboards.
---

# Mural design

This skill encodes Mural's own template design principles so that boards built through the
Mural MCP look intentional, on-brand, and usable — the way Mural's design team makes them.
Apply it to any board you construct, not just ones explicitly called "templates."

The goal in one line: **let the activity drive the layout, keep it simple, use Mural's
native defaults, and leave real room to work.**

## Order of operations

When building a board, work in this sequence. Each step depends on choices made in the one
before it.

1. **Understand the activity, then let it drive the layout.** Pick the layout the activity
   naturally wants (columns, a grid, a matrix, a left-to-right flow) rather than an abstract
   or clever arrangement. Bias toward common, recognizable layouts.
2. **Lay down the base container.** A white `area` behind almost every activity — or a
   `table` when the activity is a true grid (see Layout below).
3. **Choose the activity's colors first,** then pick complementary accents for everything else.
4. **Place content on the grid** with correct sizes, spacing, and edge margins.
5. **Add concise, grouped instructions and an action-oriented title.**
6. **Add icons from the icon tool** if they help; never rotate them.
7. **Finish:** check layering (new objects to front), confirm nothing sits on an area edge,
   and confirm there's room left for participants to actually work.

## Core principles

- **Bias toward simplicity.** Remove complexity and noise. A board that's ready to use as-is
  beats one the user has to edit down before they can start.
- **Have a clear purpose.** Every board should help someone accomplish something specific.
- **Include only essential instructions.** Give just enough to clarify each step, not more.
- **Make people look good.** People don't want to fail in front of peers. Boards should look
  good and work well so users are proud to invite others.
- **Avoid being sterile and joyless.** Without sacrificing simplicity, leave room for a little
  warmth and personality.
- **Make everything editable.** Build only from native Mural elements (areas, stickies,
  shapes, text, icons, tables) so members can change anything. Don't embed flattened images
  where a native element would do.
- **Make it modular.** Assume an activity may be used next to other activities. Keep it
  self-contained so it can be mixed and matched.
- **Design for everyone.** Legibility and contrast aren't polish — they decide whether people
  can use the board at all. Favor large text, high contrast, and signals that never rely on
  color alone. See Accessibility below.
- **Embrace the product as it is.** Design for Mural's current capabilities, not for features
  that don't exist. If a tool can't do something cleanly, choose a layout that avoids it.

## Layout and structure

- **Put a white `area` behind each activity.** The area is both the background and the semantic
  container. Use white specifically: a mural's canvas background can be dark, and white keeps
  content readable and easily movable.
- **Fill the area white and give it a gray border — every time, on the area itself.** The area
  carries its own fill and border; do *not* lay a separate white shape on top to fake the fill.
  `create_areas` has no fill or stroke parameter, so immediately after creating the area, run one
  `update_widgets` on it with `background: "#ffffff"`, `strokeColor: "#d4d4d4"` (Gray, palette row 1),
  and `strokeStyle: "solid"`. This is a reliable in-place update — verify it landed, but expect it to
  work. If the user declines the update, stop and ask.
- **Parent child widgets to the area at creation.** Pass each child's `parent_id` on its `create_*`
  call so it lands inside the area correctly the first time. Reserve `move_widgets_to_area` for
  widgets that already exist unparented — and note it *repositions* them (see the MCP reference), so
  it is a repair tool, not the normal path.
- **Use a `table`, not an area, for true grids.** If the activity is spreadsheet-like — a grid
  where users will likely add or remove rows or columns — a table is the right base layer.
  Don't simulate a grid with loose stickies on an area when a table fits.
- **Default area height is `3300px`.** Keep activities to a standard height so they can be
  mixed and matched in sequences. Width varies with the activity: wider for multiple columns,
  a matrix, many participants, side-by-side comparison, or a large synthesis space; narrower
  for something simple. Don't force small sub-sections to `3300px` — the standard applies to
  major activity areas.
- **Flow left-to-right for sequences.** In a multi-step meeting template, steps run left to
  right, with the output of one step feeding the input of the next. Put quick prep steps in the
  top-left corner.
- **Keep content off the edges.** The outer edge of an area is the grab handle users drag to
  move it. Leave a margin of at least `30px` (use `20px` only when space is genuinely tight)
  between content and the edge. Place the first meaningful content at least `30px` inside the
  bounds. This matters most for titles, instructions, stickies, participant names, icons, step
  badges, small labels, and voting dots — anything a user might try to grab or edit. But `30px` is a
  safety *minimum*, not a placement target: when a label lives in a wide margin band — an axis label
  in the strip beside a matrix grid, a caption in a gutter — center it within the band (or inset it
  generously), don't pin it to the minimum. A label parked at `30px` hugs the outer wall and leaves
  an awkward gap toward the content, so it reads as cramped even though it technically clears the
  edge. Measure the band and place the label near its middle.
- **Use lines for lines, connectors for connections.** A connector sticks to and follows other
  widgets. If you just need a divider or an axis, use a line from the Shapes tool, not a
  connector. Reserve connectors for relationships that should stay attached.
- **Order layering front-to-back.** Make sure newly added objects sit in front of backgrounds,
  not hidden behind them, before you finish.
- **Avoid locking.** Locked items confuse users. Only lock a background that must not move,
  like a radar or bullseye behind an activity.

## Dimensions and spacing

These are measured from Mural's own default tooling. Use them as the base unit system and scale
everything else around them.

- **Square sticky: `168 × 168 px`. Wide sticky: `288 × 168 px`.** Same height, so square and
  wide notes share a baseline. Treat the square sticky as the fundamental unit and scale other
  objects relative to it, rather than resizing stickies to fit a custom layout. Use the default
  size unless the activity clearly needs another scale — never resize just to look compact or
  decorative.
- **Default gap between notes: `16px` edge-to-edge.** For a grid of square stickies this means a
  **`184px` pitch** (168 + 16) center-to-center, the same horizontally and vertically. For wide
  stickies the horizontal pitch is **`304px`** (288 + 16); vertical stays `184px`.
- **`create_stickies` only enforces the `16px` gap on collision, and only against real widgets.**
  It places each note at the coordinates you give and nudges only when it would physically overlap
  another *real* widget (usually other stickies). It will *not* tidy loose or
  slightly-cramped-but-non-overlapping spacing, and areas and shapes do *not* block stickies — a
  note aimed inside a tint or band stays exactly where you put it. In a clear region, batch
  placement is pixel-exact. So place notes on the `184px` pitch yourself rather than relying on the
  tool to tidy gaps, and use `create_stickies` for batches so repeated notes stay consistent. (If a
  column comes out scrambled, suspect a collision with a concurrent editor's stickies before your
  own spacing.)
- **The spacing scale, tightest to loosest:** `16px` between related notes → `~30px` from content
  to an area edge → a larger, deliberate gap between major sections. Content-to-content should
  always be tighter than content-to-edge.
- **Use consistent internal padding.** Repeated blocks (participant workspaces, matrix cells,
  columns) must share identical padding and spacing. Uneven gaps read as sloppy.
- **Size areas for the real work, not the starter objects.** Account for the notes participants
  will add, room to cluster and move them, headings, prompts, names, legends, and any synthesis
  output. A layout that looks complete but leaves no working room has failed.

## Color

**Default to Mural's official palette for every new mural. Only use a color outside it if the
user explicitly asks for one.** The palette is 54 values in six rows; the rows are shade tiers,
and each tier has a job. Members have access to these exact colors, so a board built from them
stays consistent and fully editable.

**Which tier to use when:**

- **Row 1 — Grayscale:** text (Black `#000000` or Midnight Black `#555555`), dividers and axis
  lines (Gray `#d4d4d4`, Dark Gray `#b3b3b3`, Charcoal `#919191`), and White `#ffffff` for the
  base area behind an activity.
- **Row 2 — Light Tints:** soft background fills — activity-area accents, matrix quadrants,
  column bands, legends. Use a light tint for a soft fill rather than making a medium tone
  semi-transparent; these are the intended pastel versions.
- **Row 3 — Medium Tones:** the primary sticky-note and fill colors. Yellow `#fcf281` is the
  color a sticky gets when none is set.
- **Rows 4–5 — Dark / Darkest Shades:** emphasis, strong accents, category dots, and colored
  text that still needs contrast. Darkest is for the highest-contrast needs.
- **Row 6 — Opacity:** scrims and overlays only (e.g. dimming a background), never for content.

| Row | Colors (name `hex`) |
|---|---|
| 1 · Grayscale | Transparent `transparent` · White `#ffffff` · Light Mist `#f7f7f7` · Mist `#ededed` · Gray `#d4d4d4` · Dark Gray `#b3b3b3` · Charcoal `#919191` · Midnight Black `#555555` · Black `#000000` |
| 2 · Light Tints | Light Basil `#f9f9eb` · Light Pumpkin `#fbf3e5` · Light Vanilla `#fbf9e5` · Light Balm `#f0faeb` · Light Breeze `#effbf8` · Light Ice `#f2f8fd` · Light Orchid `#f8f7fd` · Light Blush `#fcf3f8` · Light Salmon `#fff0f0` |
| 3 · Medium Tones | Khaki `#d8d8b1` · Pumpkin `#ffd98e` · Yellow `#fcf281` · Green `#aaed92` · Teal `#86e6d9` · Blue `#9edcfa` · Violet `#d8c7ff` · Pink `#fcb6d4` · Coral `#febbbe` |
| 4 · Dark Shades | Mocha `#949474` · Orange `#f6a324` · Marigold `#e6c003` · Emerald `#459c5b` · Bermuda `#349e92` · Winter `#2897dc` · Amethyst `#9e7ee6` · Magenta `#e364a2` · Red `#e95e5e` |
| 5 · Darkest Shades | Dark Mocha `#767656` · Dark Amber `#b05b0d` · Dark Marigold `#a07602` · Dark Emerald `#147e42` · Dark Bermuda `#0f766e` · Dark Winter `#0561a6` · Dark Amethyst `#7035cc` · Dark Magenta `#be1a64` · Dark Red `#bf0c0c` |
| 6 · Opacity | White 50% `rgba(255,255,255,0.5)` · Black 10–80% `rgba(31,31,31,0.1…0.8)` |

**Common color meanings** (pick from the Medium Tones tier for stickies, matching Light Tints for
backgrounds, Dark/Darkest for emphasis):

- Good / positive / keep → **Green** `#aaed92` (bg Light Balm `#f0faeb`, strong Emerald `#459c5b`)
- Bad / negative / stop → **Coral** `#febbbe` or the stronger **Red** `#e95e5e` (bg Light Salmon
  `#fff0f0`, emphasis Dark Red `#bf0c0c`)
- Ideas → **Yellow** `#fcf281` (bg Light Vanilla `#fbf9e5`)
- Neutral categories → **Blue** `#9edcfa` / **Teal** `#86e6d9` / **Violet** `#d8c7ff`
- Accents → **Pumpkin** `#ffd98e`, **Pink** `#fcb6d4`, **Khaki** `#d8d8b1`

Deviate from these meanings only for a compelling reason. (Note: the "bad" family is Coral/Red;
Pink and Magenta are a separate accent family — don't use pink to mean "stop.")

**Themed activities may go off-palette when the user explicitly asks for a theme.** The palette is
the hard default, but an explicit themed request (e.g. a World Cup icebreaker on a green pitch with
white markings) sanctions off-palette color that serves the theme's realism. Keep it scoped to the
themed activity — don't let it bleed into standard activities on the same board — and still honor
contrast, label-carries-meaning, and tint-all-or-none.

- **Never convey meaning by color alone.** A large share of people are red-green color-blind.
  Pair color with a label, and keep any color key near the working area it explains.
- **Pick the activity's colors first,** then choose accent colors for the rest of the board that
  complement them.
- **Color-code only what's meaningful to the person, not the system's own chrome.** Don't color
  structural sections that carry no user meaning. But workflow or status *state* — kanban stage
  (To Do / In Progress / Done), RAG status, retro sentiment — is meaningful content the user reads
  and acts on, so it may carry color. Two guardrails when it does: the word always carries the
  meaning (color is reinforcement, never the sole signal), and color only the status-bearing
  element (the header or marker), not the structural band or container around it.

## Typography

- **Set text in Proxima Nova,** Mural's default typeface, unless there's a real reason not to.
- **Set explicit, bumped-up text sizes — the MCP runs small.** `create_titles` / `create_textboxes`
  render the `font_size` you pass about two steps of the "make text bigger" stepper *below* Mural's
  comfortable Heading and Paragraph defaults. Left uncorrected, a human has to select-all and click
  "make text bigger" twice on every board to fix it. Mural's UI has no numeric size field (only the
  coarse bigger/smaller stepper), so this can't be eyeballed by number — set the larger size at
  creation. Recommended starting sizes, calibrated to read comfortably at working zoom and on shared
  screens (scale the whole set proportionally for unusually large or small boards, and keep the
  hierarchy self-consistent rather than picking arbitrary per-widget sizes):
  - Activity / board title: ~`76`
  - Section, column, step, or axis heading: ~`48`
  - Subtitle / secondary heading: ~`44`
  - Paragraph, instructions, body: ~`36`
  - Small label, axis Low/High marker, caption, badge: ~`34` (these still read at working zoom;
    don't drop below ~`32`)
  Treat these as a calibrated starting scale, not hard rules — confirm once on a live board and, if
  it's off, move the whole scale together so the hierarchy stays intact. When in doubt, err larger:
  oversized text is a minor aesthetic issue, undersized text is an accessibility failure.
- **Build headers as shapes with text inside, not as colored text boxes.** When a header, section
  label, column header, or step badge needs a colored background, create a filled shape (a
  `rounded_square` or `rectangle`) and set its `text` — do *not* create a text box and color its
  background. A text box's colored region is sized by the text, so you can't control the band's
  height or width independently; the colored area ends up hugging the glyphs. A shape lets you set
  `width`, `height`, `background_color`, `stroke_color`, `stroke_size`, `font_size`, and
  `text_color` independently, so the header reads as a deliberate, consistently-sized band. Use
  `text_align: "center"` and `vertical_align: "middle"` to center the label. Set these at creation
  (shape fill and border are among the properties that can't be reliably changed later — see the MCP
  reference). Plain `create_titles` / `create_textboxes` are still right for uncolored headings and for
  body/instruction text where no background band is wanted.
- **Keep text high-contrast, and match text color to the fill it sits on.** Low-contrast text
  looks nice but is hard to read and hurts accessibility. On white, Light Tints (row 2), and Medium
  Tones (row 3), use **black** (`#000000`) or Midnight Black (`#555555`) text — these fills are
  light enough that dark text stays legible. On Dark and Darkest Shades (rows 4–5), switch to
  **white** (`#ffffff`) text — black text on a dark fill fails contrast. In short: black text is the
  default *on light fills*, but a colored header band dictates its own text color. Never leave a
  header's `text_color` to chance on a saturated fill.
- **Stick to the Markdown Mural actually renders.** Widget `text` fields take Markdown, but only a
  subset renders: **bold**, *italic*, bulleted/numbered lists, and links. Headings (`# Title`)
  render as bold text, not as a distinct heading style. Code blocks, blockquotes, and `---` rules
  are not rendered — express structure with separate widgets (titles, textboxes, shapes) instead.
- **Spell out acronyms.** A spelled-out phrase is clearer than an acronym, especially for
  non-native English speakers. Only use an acronym when it's genuinely more common than the full
  phrase (e.g. "SEO").

## Accessibility

Legibility is a design requirement, not a finishing touch. Three rules carry most of the weight:

- **Text large enough to read.** Follow the Typography scale above and err larger when unsure.
  Undersized text is the most common way a board becomes unusable for someone.
- **Enough contrast between text and its background.** Match text color to the fill tier (dark
  text on light fills, white text on dark fills — see Typography). Aim for a clear, comfortable
  contrast, not the bare minimum. This applies especially to shape-based header bands, where the
  fill color is chosen for meaning and the text color must be chosen to survive it.
- **Never signal by color alone.** Red-green color blindness is common, so any status, category,
  or sentiment that color conveys must also be carried by a word, icon, or position. Color is
  reinforcement, never the sole signal.

## Iconography

- **Use icons only from the icon tool** (`create_icons`), so members can recolor them afterward.
- **For multi-color symbols, use an emoji instead of `create_icons`.** `create_icons` returns
  single-color Noun Project silhouettes — ideal for recolorable monochrome icons, but it can't
  render flags or other multi-color glyphs. For those, put an emoji in a `create_titles` /
  `create_textboxes`; Mural rendered full-color country flags (and the England flag) this way, and
  `update_widgets` can swap a title's text, `fontSize`, or `width` in place. Caveat: flag-emoji
  color rendering is platform-dependent (Windows may show letter-pairs), so don't make it
  load-bearing.
- **Prefer chunky icons with rounded edges,** matching the style of the tools around the Mural
  canvas.
- **Aim for equal *visual* weight, not mathematical equality.** Mathematically equal sizes can
  still look unbalanced; adjust until icons feel even next to each other.
- **Never rotate icons.** Rotation breaks Mural's alignment tools and makes boards behave
  unpredictably.

## Instructions, naming, and examples

- **Group each block of instructions** so the whole top bar can be selected and deleted with one
  click — this keeps templates easy to strip down.
- **Give concise, essential instructions per step** — just enough to clarify the activity.
- **Name meeting templates with an action-oriented verb first** (Align, Decide, Create, Reflect).
  For a standalone activity with a widely accepted name, use that name; otherwise give it a short
  noun-based name. Avoid overly specific names.
- **Avoid timing in simple templates** (e.g. "5 minutes") unless the activity makes no sense
  without it.
- **Use a "think alone, think together" cadence** for sequences, and label steps accordingly.
  Don't require participant pre-work — many won't do it, and it sets the host up for failure.
  Keep prep to about ten minutes in the top-left.
- **Show what "good" looks like with examples.** Realistic examples teach a template better than
  explanation. Keep them timeless — no specific years or current events — and use realistic
  (fictional) scenarios and a consistent fictional team so examples feel real.

## Worked example: a Start / Stop / Continue retrospective for 6 people

This shows how the rules combine. Treat it as an illustration of the reasoning, not a fixed
script — the same logic scales to other activities.

**Activity drives layout:** a retro is naturally three parallel columns, so use three columns
left-to-right.

1. **Base container.** Create one `area` at `(0, 0)`, height `3300px`, width ~`3600px` (wider than
   tall because there are three columns) with `create_areas`, then immediately `update_widgets` it
   with `background: "#ffffff"`, `strokeColor: "#d4d4d4"`, `strokeStyle: "solid"` for the white fill
   and gray border. Not a table — the columns aren't a grid users add rows to.
2. **Title.** A `create_titles` reading "Start, Stop, Continue" (a widely recognized activity
   name) at ~`76px`, placed at about `(60, 60)` — comfortably inside the `30px` edge margin. If this
   were a step in a meeting template, it would instead get a verb-first name like "Reflect."
3. **Grouped instruction** in a textbox just below the title at ~`36px`, one concise line: what to
   do and in what order.
4. **Column headers as filled shapes.** Create three `rounded_square` shapes across the top of the
   working area with `create_shapes`, evenly spaced with a deliberate large gap between columns (much
   wider than the `16px` note gap, because these are major sections). Give each an explicit `width`
   and `height` so all three bands match exactly, `text` set to the column name at ~`48px`,
   `text_align: "center"`, `vertical_align: "middle"`. Color them by meaning via `background_color`:
   Start in neutral blue (`#9edcfa`), Stop in red (`#febbbe`), Continue in green (`#aaed92`) — all
   Medium Tones, so keep `text_color: "#000000"`. Because color alone isn't enough, the word itself
   is the label. (Had a header used a Dark Shade fill, its `text_color` would switch to `#ffffff`.)
   Building these as shapes rather than colored text boxes is what lets the three bands share an
   identical height and width regardless of label length.
5. **Seed stickies.** Under each header, drop a small starter cluster with `create_stickies` —
   say two or three `168px` notes stacked at a `184px` vertical pitch. Seed only a few per column
   and keep the `3300px` height mostly empty, because the point is to leave room for six people to
   add many more.
6. **Finish.** Confirm the stickies sit inside the area (not on its edge), that new widgets are
   layered above the white base, and that each column has identical padding, header size, and pitch
   so the three read as a consistent set.

The result uses only native, editable elements, follows Mural's color meanings, respects the
`16px` / `184px` / `30px` spacing scale, and — most importantly — leaves the board looking ready
to work in rather than already full.

## MCP tool quick reference

- `create_areas` → base container; **batch tool, takes an `areas` list.** Pass child widgets'
  `parent_id` at creation. It has no fill or stroke parameter, so every area needs one
  `update_widgets` follow-up setting `background: "#ffffff"`, `strokeColor: "#d4d4d4"`, and
  `strokeStyle: "solid"` (see creation-vs-update note below) — the white fill and gray border are
  standard on every activity area, not optional. If you add your own styled heading, create the area
  with no `title` (or `showTitle: false`) — passing `title=` renders a native label that then
  duplicates your heading.
- `create_stickies` → all sticky creation, single or batch; positions are absolute canvas
  coordinates even when a `parent_id` area is given; only nudges on collision (`16px` gap).
- Every batch create tool (`create_stickies`, `create_shapes`, `create_textboxes`, `create_titles`,
  `create_areas`, `create_icons`, `create_connectors`) accepts at most 100 items per call. Send a
  larger set as sequential calls of 100 or fewer and wait for each result before the next. If a
  create times out or never answers, read the board (`list_widgets`) before sending it again: it may
  already be saved, and repeating it duplicates it.
- `create_iframe_widgets` → a live embedded web page (video, document, dashboard); **batch tool,
  takes an `iframes` list.** Use it only when the page itself should be live on the board; for a
  link, use a bookmark, and for a static picture, an image. A url that cannot embed is reported in
  `failed` and the rest are still created. Iframes cannot be tagged, and a screenshot may show a
  placeholder in place of the page.
- Table tool → base layer for true add/remove-row grids, instead of an area.
- `create_shapes` → dividers, axis lines (a thin `rectangle`), background tints, **and colored
  header/label bands with text inside.** For a header band, use a `rounded_square` or `rectangle`
  with explicit `width`/`height`, a `background_color` from the palette, `text` for the label,
  `font_size` from the Typography scale, `text_color` matched to the fill tier (black on light,
  white on dark), and `text_align: "center"` / `vertical_align: "middle"`. This is preferred over a
  colored text box whenever you need control of the band's height, width, or border — a text box
  sizes its colored region to the text and can't be controlled independently. Not
  `create_connectors`, which is for attached relationships. **Shapes carry a dark default border,
  and `stroke_style: ""` does not remove it** — it only looks gone on a white fill and renders
  black on a colored fill (this is how a "white" halfway line came out black on a green pitch). To
  control a border, set `stroke_color` explicitly at creation; don't rely on clearing the style.
  For a line on a colored ground, use a thin `rectangle` with the intended color set as *both*
  fill and stroke. For a soft fill, use a Light Tint (palette row 2) rather than a
  semi-transparent medium tone; reserve the hex alpha channel / Opacity row for true overlays and
  scrims.
- `create_icons` → icons; never rotate them.
- `create_titles` / `create_textboxes` → uncolored headings and instructions, and body text; standard
  sizes, high contrast, Proxima Nova. When a heading needs a colored background band, use
  `create_shapes` with text instead (see above).

**Coordinates: create is absolute, update is area-relative.** Every `create_*` tool takes absolute
canvas x/y even when `parent_id` is set. `update_widgets`, by contrast, treats a parented widget's
x/y as relative to its parent area's top-left. Never mix the two mental models — doing so threw
stickies ~1600px off their area in one session. Place each widget correctly at creation
(`parent_id` + absolute coords) and skip the move-then-reposition dance. If you must reposition a
child later via `update_widgets`, pass *area-relative* coordinates. Moving an area moves its
children with it and preserves their offsets.

**`move_widgets_to_area` re-centers what it moves — it does not preserve position.** It's a batch tool
(takes a `moves` list of `{widget_id, area_id}`) and it places each moved widget at the area's center,
spiralling outward to avoid overlaps — it does *not* keep the widget's existing coordinates. So it is
only for reparenting widgets that already exist unparented (e.g. promoting a stopgap layout into a new
area). The two-pass repair is: (1) `move_widgets_to_area` for the whole batch to establish parenting,
then (2) one `update_widgets` pass giving each child its intended *area-relative* x/y (and size for
the fill layer). The normal path avoids all of this by parenting at creation.

**Build back to front; restack in place.** Every new widget lands above what is already on the
board, and within one create call list order is stacking order (later items on top). So create
backgrounds and tints first, then lines, then labels and stickies; in nested boxes, list the outer
box first. To fix layering afterwards, send `stack: "front"` or `stack: "back"` through
`update_widgets` — never delete and re-create a widget to change what covers what.

**Set at creation what you can't reliably change later.** Don't count on a cleanup pass. Set these
on the create call: `stroke_color` and borders on *shapes*, shape `background_color`, shape `text` /
`text_color` / `font_size`, sticky color, and position (place with `parent_id` + absolute coords).
This is why header bands built as shapes must have their fill, border, text, and text size set on the
`create_shapes` call. An area's own fill and border are the exception — they *cannot* be set on
`create_areas` (it has no fill or stroke param), so setting them is the single sanctioned
create-then-update: immediately after creating the area, `update_widgets` it with
`background: "#ffffff"`, `strokeColor: "#d4d4d4"`, and `strokeStyle: "solid"`, then verify it
landed. Don't pre-empt it with a separate white shape. If the user declines an update, stop and ask.

## Reading, verifying, and session mechanics

- **Don't trust an empty overview on a live board.** On a shared board, `get_canvas_overview` can
  report far fewer widgets than are actually present (it read 0 on a 38-widget board) — whether from
  an incomplete synchronized read or another person's concurrent edits. Before placing, confirm the real
  state of the target region with `list_widgets` on that region's aabb. Never move or delete content
  that isn't yours; relocate your own work into clear space instead.
- **Verify visually with `get_widgets_screenshot`, not the full canvas.** `get_canvas_screenshot` fails
  once a board gets large (~15,000px → "image too large"). Use `get_widgets_screenshot`, and know it
  renders **the widget IDs you pass, plus whatever is nested inside any container among them** —
  nothing else in that region is composited in. An area or table id carries its own children, so
  shooting an area shows what you put inside it; a background or fill layer that is *not* that
  area's child — a quadrant tint, a base shape at the same coordinates — still has to be listed by
  ID. Leaving out the fill layer makes the foreground float on white, which looks exactly like the
  fill failed to render; don't "fix" a fill on that basis. Before recoloring a fill that looks
  blank, re-shoot with that fill's own ID in the list, or read its `background` / `backgroundColor`
  back via `list_widgets`. A correctly placed Light Tint is intentionally near-white and renders
  fine when it's in the shot — that's the intended look, not a bug to darken. The tool is still the
  right one for catching a stray dark border on a colored fill or confirming emoji/flag rendering.
- **A card with no text is not a failed write.** Screenshots show a widget's text only up to the last saved keystroke, so a widget
  someone is typing in can come back with older or no text while the data is correct. Never re-issue the write: that duplicates the content. Read the text back with
  `get_widget_by_id` or `list_widgets`, which are authoritative, and check
  `list_selected_widgets` for `editing: true` to confirm the cause.
- **Pin the session to the board once.** `select_mural` needs the workspace-prefixed ID
  (`murally-org.<number>`), not the bare number from the URL. Once selected, the choice is stored in
  session state, so downstream canvas tools can omit `mural_id`.

## A note on matrices and consistency

When a layout has repeated cells — a 2×2 matrix, a set of columns, parallel participant
workspaces — treat the cells as a set: they must look consistent. **Tint all cells or none.**
Leaving some cells colored and others blank makes the whole grid look unbalanced and unfinished,
even when every individual cell is fine. The same applies to borders, padding, header band size,
and label placement: whatever you do to one repeated cell, do to all of them.
