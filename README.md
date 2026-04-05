# Perxel Blocks

A **[Claude Code](https://claude.ai/code) plugin** for building **custom** WordPress blocks alongside [NectarBlocks by ThemeNectar](https://nectarblocks.com) — scaffold, audit, and manage ACF-based Gutenberg blocks with a structured, production-ready workflow.

## Install

**From GitHub (recommended):**
```bash
# Add the marketplace once
claude plugin marketplace add perxel/perxel-blocks

# Install the plugin
claude plugin install perxel-blocks@perxel --scope project
```

**From official Claude marketplace** (if listed):
```bash
claude plugin install perxel-blocks@claude-plugins-official --scope project
```

**Local install (for testing or offline use):**

1. Clone the repo
```bash
git clone https://github.com/perxel/perxel-blocks.git
```

2. Register the local folder as a marketplace (run once)
```bash
claude plugin marketplace add /your/path/to/perxel-blocks
```

3. Install from it
```bash
claude plugin install perxel-blocks@perxel --scope project
```

## Update

```bash
claude plugin update perxel-blocks@perxel
```

**Local install** — pull the latest and reinstall:
```bash
cd /your/path/to/perxel-blocks && git pull
claude plugin install /your/path/to/perxel-blocks --scope project
```

> Plugin updates are not automatic. Always bump the `version` in `.claude-plugin/plugin.json` when publishing changes — Claude Code uses the version number to detect updates.

## Commands

| Command | What it does |
|---|---|
| `/perxel-blocks:init <project-name>` | Scaffold a new WordPress theme with full perxel-blocks setup |
| `/perxel-blocks:create-block <name>` | Create a new ACF block with all required files |
| `/perxel-blocks:audit-blocks` | Audit all blocks against conventions |
| `/perxel-blocks:audit-blocks-deep` | Deep AI audit — content quality, labels, admin render justification |

## Agent

The `perxel-blocks-dev` agent has full knowledge of the workflow built in. Ask it anything about block structure, PHP patterns, ACF fields, asset loading, or Tailwind setup — no context needed.

## Optional features

The core workflow is PHP-first. The following are opt-in during `/perxel-blocks:init`:

- **Tailwind v4** — CSS build pipeline with per-developer CSS support
- **TypeScript + tsup** — JS bundler for npm packages and block scripts
- **Per-developer CSS** — each developer builds their own Tailwind output independently

## Documentation

- [Introduction](docs/INTRODUCTION.md) — what perxel-blocks is and who it is for
- [Workflow](docs/WORKFLOW.md) — step-by-step development guide
- [Conventions](docs/CONVENTIONS.md) — block structure rules and patterns

## Requirements

- WordPress 6.0+
- ACF Pro
- PHP 8.0+
- Node.js + pnpm (if using Tailwind or TypeScript)

## License

MIT — [perxel](https://github.com/perxel)
