---
name: troubleshooting
description: "Recovery guide for Mural MCP canvas tool errors: unknown or renamed tool names, Canvas Host synchronization and capacity errors, auth and access failures, view-only and archived refusals, no mural selected, concurrency and batch limits, uncertain writes, daily usage limits, viewport and selection reads that need an open Mural app, screenshot errors, iframe creation, and per-tool failures, with when a retry is safe. Also covers successful calls that read as failures: a blank card or an iframe placeholder in a screenshot."
---

# Troubleshooting

Canvas tools load and edit murals through Canvas Host. No separate interactive
canvas session is needed. Failures should be handled from the returned
`error_code`, `error_message`, and any execution or retryability metadata.

Errors surface in two shapes:

- **A raised tool error.** Most of these messages are written for the user:
  relay them in your own words and use the matching section below. Three are
  addressed to you, so act on them instead: "Unknown tool" (see the next
  section), "No mural_id provided and no active mural selected" (see "No mural
  selected or wrong mural"), and `DAILY_LIMIT_REACHED` (see "Daily usage limit
  reached").
- **A `success: false` result dict** with `error_code` and `error_message`
  fields. Read the code before deciding whether a retry is safe.

## "Unknown tool" errors

**"Unknown tool"** / **"tool not found"** means you called a name this server
does not serve — usually one that was renamed. This server raises it while
resolving the name, before any tool runs, so it carries no `error_code`.

**Find the replacement yourself; do not ask the user to reconnect.** The tool
you want is almost always already in your list under a different name, so
searching it costs you one step where a reconnect costs the user several — and
some clients reuse the catalog they opened with, so it may not even work.

Re-read your tool list and look for the tool that serves the same purpose. Two
patterns cover most renames here:

- **A plural form of the same name.** Writers were consolidated into batch
  tools, so a singular `create_x` is now `create_xs`, taking a list. Pass the
  arguments you already built as its single entry — reusing them unchanged
  gives a missing-argument error instead.
- **A different verb for the same read.** Readers were renamed toward
  `list_*` and `get_*_screenshot`.

Failing both, call `list_skills` and pick the closest match by purpose. Do not
retry the failed name.

## Canvas Host synchronization and capacity

- `STALE_READ`: the host is synchronizing authoritative mural state. Wait
  briefly and retry the read.
- `CONTEXT_NOT_LOADED`: Canvas Host could not load the mural. It already
  retried once on its own; if the error still surfaces, wait briefly and retry
  once.
- `POD_SATURATED`: Canvas Host has no capacity for another mural right now, or
  it is restarting. Wait a little, then retry. Avoid opening many murals at
  once.
- `CANVAS_HOST_UNAVAILABLE`: the Canvas Host could not be reached for this
  command. The message says whether the change may already have been applied:
  if it says retrying is safe, wait a few seconds and retry; otherwise read the
  mural before retrying a write. Report it if it keeps failing.
- `CALL_TIMEOUT`: reconcile before retrying a write because its outcome may be
  unknown. A read can be retried after the host becomes responsive.
- `CALL_OUTCOME_UNKNOWN`: the command was received and then went unanswered,
  so a write may or may not have been applied. Read the mural before
  deciding whether to issue it again.
- `PERSIST_FAILED`: the mutation was not confirmed saved. Re-read authoritative
  state before deciding whether to issue it again.
- `WRITE_REJECTED_ASYNC`: the host is reconciling a backend rejection. Wait for
  synchronization, re-read the mural, then retry only if the intended change is
  absent.

Never blindly retry an uncertain write. When the response does not prove that a
mutation was not executed, read the mural and reconcile first.

## Authentication and access errors

- `AUTH_EXPIRED` / `AUTH_REVOKED`, **"Authentication failed"**, **"No access
  token available"**, or **"No user ID in token claims"**: disconnect and
  reconnect the MCP client, then complete MURAL authorization again.
- **"Access denied"**: the account lacks access to the mural, or the OAuth token
  lacks the required scopes. Confirm the account and mural id, then reconnect
  the MCP client to obtain current scopes.
- **"Mural <id> not found"**: the id is wrong or inaccessible. Call
  `select_mural()` or use `get-mural-id/SKILL.md` to derive it again.

## View-only mural

`MURAL_READ_ONLY` means the user has view-only access, so the write was
refused. Every write that draws on the canvas refuses the same way. Do not
retry: this is a permission decision, not a transient failure.

Tell the user they have view-only access and the write was refused. Edit access
has to come from an editor, and no tool of yours grants it. Once an editor
grants it, the next write lands.

A batch that commits item by item can have landed some items before the access
change refused the rest. Read the mural before redoing the work. Reads still
work.

## Archived mural

`MURAL_ARCHIVED` means the mural is archived, so the write was refused. Every
write that draws on the canvas refuses the same way. Do not retry: this is a
permission decision, not a transient failure.

Tell the user the mural has to be unarchived in Mural before anything can be
added to it. No tool of yours does that, so do not offer to. Once they
unarchive it, the next write lands. A batch that commits item by item can have
landed some items first, so read the mural before redoing the work. Reads still
work.

## No mural selected or wrong mural

If a tool reports **"No mural_id provided and no active mural selected"**, call
`select_mural()` with no arguments, ask the user which candidate they mean, and
then call `select_mural(mural_id=<id>)`. The host loads that mural on the first
canvas operation.

## Too many parallel calls

`REALTIME_TOO_MANY_IN_FLIGHT` means too many operations are already running for
the same mural. Do not immediately duplicate the command. Wait for earlier calls
to return, then serialize later operations for that mural.

## Too many items in one call

If a result has `error_code: BATCH_TOO_LARGE`, the call asked for more items
(or more text) than one call accepts, and nothing was created or changed. The
create tools take at most 100 items per call. Split the work into sequential
calls inside the bound the message names, waiting for each result before
sending the next.

## A write timed out or never answered

A call that changes the mural can be saved even when you never see its answer:
it timed out, ended with an unclear outcome such as `CALL_OUTCOME_UNKNOWN`, or
returned nothing. **Do not repeat it straight away.** Read the board first
(`get_widget_by_id` for ids you know, `list_widgets` otherwise) and repeat only
what is missing; repeating a create that already landed duplicates its widgets.
Keep each call small (at most 100 items) so it answers in time.

Writes outside the board need a different check. After `create_mural`, look
for the new title with `search_murals` before creating it again.
`invite_users_to_mural` and `submit_feedback` have no read that shows whether
they landed, so do not repeat them; tell the user the outcome is unknown.

## Daily usage limit reached

A raised tool error starting `DAILY_LIMIT_REACHED` means the user has used up a
daily usage limit: either the daily total of Mural tool calls, or a shared limit
on one group of tools, such as screenshots. It arrives as a raised tool error,
not as a `success: false` result, and its message names the limit that was
reached and when it resets.

- **Do not retry.** The refusal lasts until the limits reset at 00:00 UTC, and
  nothing you do before then lifts it.
- **Refused requests still count.** Once a group limit such as screenshots is
  reached, every further request for that group is refused and still spends the
  daily total, so repeating it can use up the limit on every other tool too.
- If only a group limit was reached, carry on with other tools: for screenshots,
  read the board with `list_widgets` or `get_widget_by_id` instead. If the daily
  total was reached, tell the user in your own words that today's limit is used
  up and when it resets, and wait for the reset.

## Screenshot dimension errors

`SCREENSHOT_DIMENSIONS_EXCEEDED` means the requested output size is invalid.
Follow the returned `retry_guidance.suggestion`. The simplest correction is to
omit `output_width` and `output_height` and let the tool fit the explicit region
to `max_output_dimension` while preserving aspect ratio.

## Canvas Host operation constraints

- `list_selected_widgets` answers with what the user has selected in the
  Mural app they have open on this mural. With none open its list is empty and
  `message` says so; that is the answer, not a failed read.
- `list_widgets_in_viewport`, and `get_viewport_screenshot` without a `region`,
  read the area the user is looking at in the Mural app they have open on this
  mural. `NO_OPEN_VIEW` means either no Mural app is open on this mural (the
  error carries a link that opens it), or an open app has not shared its view
  yet; in the second case, retry in a moment or ask the user to pan once.
  `VIEW_CROWDED` means the
  mural is too crowded for apps to share their view. Otherwise use
  `list_widgets` with an explicit `aabb`, pass the screenshot a `region`, or use
  `get_canvas_screenshot` for the whole board.
- `OP_NOT_ALLOWED` means Canvas Host does not support the requested
  operation. Choose a supported canvas tool rather than retrying the same call.
- `MURAL_MODE_BLOCKED` means a voting or private-mode state temporarily blocks
  writes. Retry only after that mode ends.

## A screenshot shows old or missing text

Screenshots render the saved widget data, so a widget someone is typing in shows
its text only up to the last saved keystroke and can come back blank. A blank
card is not a failed write.

**Do not re-write the content**: that duplicates it. Instead, read its text with
`get_widget_by_id` or `list_widgets`, which are authoritative for content. To
confirm the cause, call `list_selected_widgets`: a widget someone is editing
carries `editing: true`. If so, leave it alone and re-shoot later, or take the
structured read as your verification.

## An iframe shows as a placeholder in a screenshot

Also not an error. A screenshot may draw an iframe widget as a placeholder
instead of the embedded page. Do not re-create the iframe on this evidence.
Confirm it with `list_widgets` or `get_widget_by_id`, which report the
`iframe_url`, `source_url` and `page_title`, and take that as your
verification.

## `create_iframe_widgets` did not create an iframe

- **`IFRAME_URL_NOT_EMBEDDABLE`**: the page refuses to be embedded. Nothing was
  created for that url, and the other urls in the call were. Use the bookmark or
  image the failure message suggests instead of retrying the same url. When
  every url in the call fails this way, embeds may be turned off for the mural:
  tell the user rather than trying other urls.
- **`INVALID_URL`**: the url is malformed. Fix it and resend only that url.
- **`IFRAME_EMBEDS_UNAVAILABLE`**: embeds are turned off for this mural: a
  feature flag is off. Nothing was created. Retrying does not help; tell the
  user, and offer a bookmark or image.
- **`IFRAME_CREATE_FAILED`**: the canvas could not create those iframes. Follow
  the message's recovery steps; re-check the canvas before retrying when it
  says the iframes may have landed.
- Iframes cannot be tagged, so `add_tag` on one is expected to fail.

## Per-tool operation failures

Codes such as `CREATE_STICKIES_FAILED`, `UPDATE_WIDGETS_FAILED`,
`CANVAS_WIDGETS_READ_FAILED`, `PERMISSIONS_READ_FAILED`, and `TAG_ADD_FAILED`
mean the canvas operation was rejected. Common causes are insufficient
permissions, a stale target id, or invalid arguments. Re-read current ids and
permissions, correct the request, and retry only when the failure is known to be
safe to retry.
