# The Patchbowl Catalog

This directory is the source of truth for every MCP server Patchbowl knows about. Each server is one YAML file. Adding a server is one PR.

## Add a server in three steps

1. **Copy the template:**
   ```bash
   cp catalog/_template.yaml catalog/<server-name>.yaml
   ```
2. **Fill in the fields** (see schema below).
3. **Open a PR.** A maintainer will review within ~24 hours.

That's it. No code. No build step. No tests required (we run schema validation in CI).

## Schema

```yaml
# catalog/<server-name>.yaml

# Required ---------------------------------------------------------------
id: github-mcp-server               # unique slug, kebab-case
name: GitHub                        # display name
description: >                      # one or two sentences, plain English
  Read and write GitHub repos, PRs, and issues from your AI client.
category: dev-tools                 # see categories below
source:
  type: npm                         # npm | pypi | docker | github
  package: "@modelcontextprotocol/server-github"
  # or for github source: repo: "owner/repo"
runtime:
  command: npx                      # the command your client will run
  args: ["-y", "@modelcontextprotocol/server-github"]
license: MIT                        # SPDX identifier

# Optional but recommended ------------------------------------------------
homepage: https://github.com/owner/repo
maintainer: "@owner"                # GitHub handle of the upstream maintainer
secrets:                            # what env vars this server needs
  - key: GITHUB_PERSONAL_ACCESS_TOKEN
    description: "PAT with repo scope"
    required: true
tags: ["github", "git", "code"]
verified: false                     # true only after security review
```

## Categories

Pick the one closest. We can refine later.

- `dev-tools` — code, git, CI/CD, package managers
- `data` — databases, file systems, blob storage
- `productivity` — notes, calendars, email, docs
- `web` — browsers, scrapers, search
- `ai` — embeddings, vector stores, model gateways
- `infra` — cloud, containers, observability
- `other`

## What makes a server worth adding

We aim for a *curated* catalog, not an exhaustive one. Good candidates:

- ✅ Actively maintained (commits in the last 3 months)
- ✅ Clear license (MIT, Apache-2.0, AGPL, etc.)
- ✅ Documented configuration (knows its own env vars)
- ✅ Works on macOS *and* Linux

Likely-to-reject:

- ❌ Single-author abandoned repos with no commits in a year
- ❌ No license file
- ❌ Embeds hardcoded credentials in source
- ❌ Duplicates an existing catalog entry without meaningful difference

## Updating an existing server

PRs welcome to bump versions, fix descriptions, add tags, or mark a server unmaintained.

## Questions?

Open a [Discussion](../../discussions) or file an [Add-a-server issue](../../issues/new?template=add-server.yml).
