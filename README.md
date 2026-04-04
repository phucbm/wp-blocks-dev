# nectarblocks

A Claude Code plugin for building **custom** WordPress blocks alongside [NectarBlocks by ThemeNectar](https://nectarblocks.com) — scaffold, audit, and manage ACF-based Gutenberg blocks with a structured, production-ready workflow.

> Not affiliated with ThemeNectar or [nectarblocks.com](https://nectarblocks.com). Built to support developers who extend NectarBlocks with their own custom blocks.

## Install

**From marketplace:**
```bash
claude plugin install nectarblocks@perxel --scope project
```

**Local install (for testing or offline use):**

1. Clone the repo
```bash
git clone https://github.com/perxel/nectarblocks.git
```

2. Get the absolute path
```bash
cd nectarblocks && pwd
```

3. Register the local folder as a marketplace (run once)
```bash
/plugin marketplace add /Users/bmp/PHUC-LOCAL/perxel/nectarblocks
```

4. Install from it
```bash
/plugin install nectarblocks@nectarblocks --scope project
```

## Update

**From marketplace:**
```bash
claude plugin update nectarblocks@perxel
```

**Local install** — pull the latest and reinstall:
```bash
cd /your/path/to/nectarblocks && git pull
claude plugin install /your/path/to/nectarblocks --scope project
```

> Plugin updates are not automatic. Always bump the `version` in `.claude-plugin/plugin.json` when publishing changes — Claude Code uses the version number to detect updates.

## Commands

| Command | What it does |
|---|---|
| `/nectarblocks:init <project-name>` | Scaffold a new WordPress theme with full nectarblocks setup |
| `/nectarblocks:create-block <name>` | Create a new ACF block with all required files |
| `/nectarblocks:audit` | Audit all blocks against conventions |
| `/nectarblocks:enqueue <handle>` | Add a JS or CSS asset to the enqueue system |

## Agent

The `nectarblocks-dev` agent has full knowledge of the workflow built in. Ask it anything about block structure, PHP patterns, ACF fields, asset loading, or Tailwind setup — no context needed.

## Optional features

The core workflow is PHP-first. The following are opt-in during `/nectarblocks:init`:

- **Tailwind v4** — CSS build pipeline with per-developer CSS support
- **TypeScript + tsup** — JS bundler for npm packages and block scripts
- **Per-developer CSS** — each developer builds their own Tailwind output independently

## Documentation

- [Introduction](docs/INTRODUCTION.md) — what nectarblocks is and who it is for
- [Workflow](docs/WORKFLOW.md) — step-by-step development guide
- [Conventions](docs/CONVENTIONS.md) — block structure rules and patterns

## Requirements

- WordPress 6.0+
- ACF Pro
- PHP 8.0+
- Node.js + pnpm (if using Tailwind or TypeScript)

## License

MIT — [perxel](https://github.com/perxel)
