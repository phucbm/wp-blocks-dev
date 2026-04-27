# WP Blocks Dev
![wp-blocks-dev-claude-plugin](https://github.com/user-attachments/assets/76762c04-780d-4ab4-9c82-5f91cbe94225)

A **[Claude Code](https://claude.ai/code) plugin** for scaffolding, auditing, and managing custom **ACF Gutenberg blocks
** for WordPress — with Tailwind CSS integration and a structured, production-ready workflow.


## Example use cases

**Example 1: Starting a new WordPress project**

Run `/wp-blocks-dev:init my-client-site` — the plugin asks for project name, theme slug, author, and which optional
features (Tailwind, TypeScript) to include, then scaffolds the full theme structure.

**Example 2: Adding a new block**

Run `/wp-blocks-dev:create-block hero-banner` — generates `block.json`, `render.php`, `fields.json`, and registers the
block in `blocks.json` automatically.

**Example 3: Auditing before a handoff**

Run `/wp-blocks-dev:audit-blocks` — instantly checks all blocks for missing files, invalid timestamps, unregistered
folders, and render issues. No AI, runs in seconds.

<img width="1173" height="340" alt="audit-blocks" src="https://github.com/user-attachments/assets/1802a04f-293b-4720-892c-6f3d55c0791a" />

**Example 4: Deep content review**

Run `/wp-blocks-dev:audit-blocks-deep` — AI reviews block labels, field quality, admin render justification, and content
consistency across the project.

<img width="1309" height="882" alt="audit-blocks-deep" src="https://github.com/user-attachments/assets/18ed1a36-f88b-4c93-a120-6fb80467e68e" />


**Example 5: Asking the agent**

Talk to the `wp-blocks-dev` agent when you're unsure about block structure, ACF field setup, PHP patterns, or
Tailwind config — no need to provide context, it knows the workflow.

## Install

**From GitHub (recommended):**

1. Add the marketplace once
```bash
claude plugin marketplace add phucbm/wp-blocks-dev
```

2. Install the plugin
```bash
claude plugin install wp-blocks-dev@phucbm --scope project
```

**Local install (for testing or offline use):**

1. Clone the repo
```bash
git clone https://github.com/phucbm/wp-blocks-dev.git
```

2. Register the local folder as a marketplace (run once)
```bash
claude plugin marketplace add /your/path/to/wp-blocks-dev
```

3. Install from it
```bash
claude plugin install wp-blocks-dev@phucbm --scope project
```

## Update

```bash
claude plugin update wp-blocks-dev@phucbm
```

**Local install** — pull the latest and reinstall:
```bash
cd /your/path/to/wp-blocks-dev && git pull
claude plugin install /your/path/to/wp-blocks-dev --scope project
```

> Plugin updates are not automatic. Always bump the `version` in `.claude-plugin/plugin.json` when publishing changes — Claude Code uses the version number to detect updates.

## Commands

| Command | What it does |
|---|---|
| `/wp-blocks-dev:init <project-name>` | Scaffold a new WordPress theme with full wp-blocks-dev setup |
| `/wp-blocks-dev:create-block <name>` | Create a new ACF block with all required files |
| `/wp-blocks-dev:audit-blocks` | Audit all blocks against conventions |
| `/wp-blocks-dev:audit-blocks-deep` | Deep AI audit — content quality, labels, admin render justification |

## Agent

The `wp-blocks-dev` agent has full knowledge of the workflow built in. Ask it anything about block structure, PHP patterns, ACF fields, asset loading, or Tailwind setup — no context needed.

## Optional features

The core workflow is PHP-first. The following are opt-in during `/wp-blocks-dev:init`:

- **Tailwind v4** — CSS build pipeline with per-developer CSS support
- **TypeScript + tsup** — JS bundler for npm packages and block scripts
- **Per-developer CSS** — each developer builds their own Tailwind output independently

## Documentation

- [Introduction](docs/INTRODUCTION.md) — what wp-blocks-dev is and who it is for
- [Workflow](docs/WORKFLOW.md) — step-by-step development guide
- [Conventions](docs/CONVENTIONS.md) — block structure rules and patterns

## Requirements

- WordPress 6.0+
- ACF Pro
- PHP 8.0+
- Node.js + pnpm (if using Tailwind or TypeScript)

## License

MIT — [phucbm](https://github.com/phucbm)
