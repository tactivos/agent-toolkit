---
name: get-mural-id
description: Instructions for how to retrieve the `mural_id` (e.g. `mural2498.1777661473160`) necessary for using canvas-related Mural MCP tools. Read before using any Mural-related MCP tools.
---

# Finding the mural id

Every canvas tool needs a `mural_id` argument (e.g. `mural2498.1777661473160`).

## Preferred: call the `select_mural` tool

1. Call `select_mural()` with no arguments. The response includes a `candidates` list of the user's recently-opened murals (most-recent first).
2. Show the candidates to the user and ask which mural to use. **Never guess** — the user may have multiple murals and only they know which one is intended.
3. Call `select_mural(mural_id=<chosen id>)` to persist the choice for this MCP session. Canvas Host loads the selected mural on the first canvas operation.

Once selected, the choice is stored in session state. Canvas tools that accept `mural_id` as optional (e.g. `create_stickies`) can be called without it. Other tools still need `mural_id` passed explicitly — read it from `select_mural`'s response.

If `select_mural` returns no candidates (the user has no recently-opened murals, or the Public API is unavailable), fall back to the methods below.

## When the user names the mural

`search_murals(query=<words from the title>)` finds murals the user has not opened recently, across every workspace they belong to. Matching covers the mural's **title** only — canvas content is not indexed, so words the user remembers seeing on a sticky will not find it. Whole title words match in any order, and only the last word also matches as a prefix ('road' finds 'Roadmap', 'road map' does not). One matching word is enough — so extra words widen the results rather than narrowing them, and a single distinctive word works better than a remembered full title. Mid-word fragments match nothing and typo tolerance is not guaranteed, so an empty result is worth retrying with a shorter word rather than a different spelling. Each result carries a `mural_id` you can pass to `select_mural` to pin it for the session.

## Extracting from a Mural URL

The easiest way to get a `mural_id` is to pull it from a Mural's URL. **When in doubt, ASK the user for the URL to the Mural.**

Given a Mural URL, like  

```
https://[some env].mural.co/t/murally-org/m/murally-org/1778789992824/[SHA-1 hash]
```

the `mural_id` is the two path segments after the `m/` concatenated with a `.`:

```
murally-org.1778789992824
```

