<div align="center">

# 🥣 Patchbowl

**Patches for the messiest parts of building with AI.**

Self-host one control plane for all your MCP servers.
Install once, sync everywhere, never edit a config file again.

[![Deploy to DO](https://www.deploytodo.com/do-btn-blue.svg)](https://cloud.digitalocean.com/apps/new)

![Stars](https://img.shields.io/github/stars/patchbowl/patchbowl?style=social)
![License](https://img.shields.io/badge/license-AGPL--3.0-blue.svg)
![Status](https://img.shields.io/badge/status-pre--alpha-orange)

[Demo](#) · [Docs](#) · [Discord](#) · [Roadmap](#roadmap)

</div>

---

## The problem

You're building with AI. You use Claude Desktop *and* Cursor *and* Cline. Every one of them speaks MCP. Every one of them has its own JSON config file in its own weird location. Every server you install means editing all of them by hand, pasting API keys in plaintext, and praying.

When something breaks, there are no logs. When you upgrade, nothing tells you. When a teammate joins, you send them a Notion doc.

This is the patchbowl. The mess where patches go to die.

## What Patchbowl does

Patchbowl is one self-hosted control plane that owns the truth about *your* MCP servers.

- **Install once, sync everywhere.** Pick a server from the catalog → Patchbowl writes it into Claude Desktop, Cursor, Cline, Windsurf, and Zed in one go.
- **Secrets that aren't plaintext.** API keys are stored encrypted, injected at runtime, never written to your client configs.
- **Health you can see.** Know which servers are running, which crashed, and why. Logs in one place.
- **A catalog you can trust.** Curated, community-PR'd, version-pinned MCP servers — not random GitHub repos.
- **Your machine, your rules.** Single binary or Docker Compose. No telemetry by default. No account required.

## Quickstart

```bash
# Docker (coming soon)
docker run -d -p 3000:3000 -v patchbowl:/data ghcr.io/patchbowl/patchbowl

# Or one-click on a droplet — see docs/deploy/digitalocean.md
```

Open `http://localhost:3000`, pick servers from the catalog, click install. Done.

> ⚠️ **Status: pre-alpha.** We're in the open from day one. Star the repo to follow along — the first usable release lands in a few weeks. Want to help shape it? [Open an issue](../../issues/new/choose) or hop in Discord.

## How it works

```
                      ┌──────────────────┐
                      │    Patchbowl     │
                      │  (your droplet   │
                      │   or laptop)     │
                      └────────┬─────────┘
                               │ writes configs
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
     ┌───────────────┐ ┌──────────────┐ ┌──────────────┐
     │ Claude Desktop│ │    Cursor    │ │ Cline / Zed  │
     │   (config)    │ │   (config)   │ │  (configs)   │
     └───────────────┘ └──────────────┘ └──────────────┘
```

Patchbowl is the source of truth. Your clients are read replicas.

## What's in the bowl (v1)

- [ ] MCP server catalog (target: 100+ servers at launch)
- [ ] One-click install across Claude Desktop, Cursor, Cline, Windsurf, Zed
- [ ] Encrypted secret storage
- [ ] Self-host via Docker or one-click DigitalOcean
- [ ] Health checks and logs

## Roadmap

- 🔜 Team mode — share a Patchbowl with your team, RBAC for secrets
- 🔜 Server audit — automated security scoring for each catalog entry
- 🔜 Custom server builder — wrap any HTTP API as an MCP server from the UI
- 🔜 More patches — beyond MCP. Prompt packs, agent skills, model configs.

## Self-host on DigitalOcean

A `$6/mo` basic droplet runs Patchbowl comfortably. Full guide: [`docs/deploy/digitalocean.md`](docs/deploy/digitalocean.md).

## Why "Patchbowl"?

Because the MCP ecosystem is a bowl of half-stuck patches right now — and we're going to clean it up, one server at a time. The plan is to keep going: every messy corner of AI development gets a patch in the bowl.

## Contributing

Adding a server to the catalog is one PR with one YAML file — see [`catalog/README.md`](catalog/README.md). We label good-first-issues weekly. Join the Discord, say hi, ship something.

Full contribution guide: [`CONTRIBUTING.md`](CONTRIBUTING.md).

## License

[AGPL-3.0](LICENSE) — free to self-host forever, share-alike if you fork.
