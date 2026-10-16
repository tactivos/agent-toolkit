# AGENTS.md

## This repository is public

Anyone on the internet can read everything in this repo: files, commit
messages, branch names, pull requests, review comments, issues, and release
notes. Write all of it as if a stranger outside Mural will read it, because
one will.

## Never reference private implementation details

Mural's other repositories are private. Do not reveal how they work
**anywhere** in this repo. That includes:

- Commit messages and branch names
- PR titles, descriptions, and review comments
- Issues and issue comments
- `README.md`, this file, and any other docs
- Code comments, manifests (`plugin.json`, `mcp.json`, etc.), and assets

"Implementation details" includes, but is not limited to:

- Names, paths, or URLs of private repositories, files, modules, classes,
  functions, or tests
- Code copied or paraphrased from a private repo
- Internal architecture: how services talk to each other, what calls what,
  where data is stored, how auth or rate limiting works behind the scenes
- Internal hostnames, environments, endpoints, infrastructure, or
  deployment details
- Feature flag names, environment variables, config keys, or secrets
- Logs, metrics, dashboards, traces, or incident details
- Unreleased features, internal roadmaps, or customer names

## What you can say

Describe changes in terms of what is already public: the files in this repo,
the public Mural MCP server address in `mcp.json`, public Mural docs and
help articles, and the behavior a user can see from their agent.

If a reviewer needs private context to understand a change, put it somewhere
private (the internal ticket, or a PR in the private repo) and keep the
public text to what is visible from here.

## Check before you push

Before every commit, push, PR, or comment, reread what you wrote and ask:
could someone outside Mural learn anything about Mural's private code or
systems from this? If so, rewrite it. Git history is permanent and public;
an amended commit or edited PR description can still be found later, so get
it right the first time.
