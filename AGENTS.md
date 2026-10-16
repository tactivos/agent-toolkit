# AGENTS.md

## This repository is public

Anyone on the internet can read everything in this repo: files, commit
messages, branch names, pull requests, review comments, issues, and release
notes. Write all of it as if a stranger outside Mural will read it, because
one will.

The rest of Mural's code, systems, and planning is private. Nothing internal
belongs here.

## Where this applies

Everywhere text ends up in this repo or on its GitHub page:

- Commit messages and branch names
- PR titles, descriptions, and review comments
- Issues and issue comments
- `README.md`, this file, and any other docs
- Code comments, manifests (`plugin.json`, `mcp.json`, etc.), and assets

## What to leave out

### Private code and systems

- Names, paths, or URLs of private repositories, files, modules, classes,
  functions, or tests
- Code copied or paraphrased from a private repo
- Internal architecture: how services talk to each other, what calls what,
  where data is stored, how auth or rate limiting works behind the scenes
- Internal hostnames, environments, endpoints, infrastructure, or
  deployment details
- Feature flag names, environment variables, config keys, or secrets
- Logs, metrics, dashboards, traces, or incident details

### Customer and personal data

- Customer or company names
- User names, emails, or account, workspace, or mural IDs
- Screenshots or exports of real murals or workspaces

### Business-confidential information

- Unannounced features, integrations, or partners
- Vendor or partner dealings, including anything under NDA
- Pricing, plans, usage numbers, roadmaps, or release dates that aren't
  already public

### Security issues

Don't describe vulnerabilities, auth weaknesses, or abuse scenarios in public.
Report them privately to the Mural team instead.

### Internal links

Don't link to internal tools (issue trackers, chat, wikis, shared docs,
monitoring). Outsiders can't open them, and the URL often contains the page
or ticket title. A bare ticket ID (for example `ABC-123`) is fine; its title,
link, and contents are not.

### Your own working files

Local paths (such as `/Users/...`), plan documents, scratch notes, and
transcript excerpts from your session don't belong in commits, PRs, or docs.

### Content with an unclear license

Don't add third-party code or copy internal docs, skills, or prompts into this
repo unless a human has confirmed they can be published here.

## What you can say

Describe changes in terms of what is already public: the files in this repo,
the public Mural MCP server address in `mcp.json`, public Mural docs and
help articles, and the behavior a user can see from their agent.

If a reviewer needs private context to understand a change, put it somewhere
private (the internal ticket, or a PR in the private repo) and keep the
public text to what is visible from here.

## Check before you publish

Before every commit, push, PR, or comment, reread what you wrote and ask:
could someone outside Mural learn anything internal from this? If so,
rewrite it. If you aren't sure, leave it out and ask a human.

Git history is permanent and public. An amended commit or edited PR
description can still be found later, so get it right the first time.
