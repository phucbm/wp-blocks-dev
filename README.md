# WP Blocks Dev

A **[Claude Code](https://claude.ai/code) plugin** for scaffolding, auditing, and managing custom ACF Gutenberg blocks for WordPress — with Tailwind CSS integration and a structured, production-ready workflow.

## Install

**From GitHub:**

```bash
claude plugin marketplace add phucbm/wp-blocks-dev
claude plugin install wp-blocks-dev@phucbm --scope project
```

**Local install:**

```bash
git clone https://github.com/phucbm/wp-blocks-dev.git
claude plugin marketplace add /your/path/to/wp-blocks-dev
claude plugin install wp-blocks-dev@phucbm --scope project
```

## Update

```bash
claude plugin update wp-blocks-dev@phucbm
```

> Always bump `version` in `.claude-plugin/plugin.json` when publishing — Claude Code uses it to detect updates.

---

## Skills

| Skill | Example prompt | What it does |
|---|---|---|
| `/wp-blocks-dev:init` | `/wp-blocks-dev:init my-client-site` | Scaffold a new WordPress theme — asks for project name, slug, author, and optional features (Tailwind, TypeScript), then creates the full theme structure |
| `/wp-blocks-dev:create-block` | `/wp-blocks-dev:create-block hero-banner` | Scaffold a new ACF block — generates `block.json`, `fields.json`, `render.php`, and registers it in `blocks.json` |
| `/wp-blocks-dev:audit-blocks` | `/wp-blocks-dev:audit-blocks` | Fast structural audit — checks all blocks for missing files, invalid timestamps, unregistered folders. No AI, runs in seconds |
| `/wp-blocks-dev:audit-blocks-deep` | `/wp-blocks-dev:audit-blocks-deep` | Deep AI audit — reviews block labels, field quality, admin render justification, and empty-state messages across the project |

## Agent

| Agent | Example prompt | What it does |
|---|---|---|
| `wp-blocks-dev` | `@wp-blocks-dev should this block have render-admin.php?` | Expert on the full stack — ask anything about block structure, ACF fields, PHP patterns, asset loading, or Tailwind. No context needed, it knows the workflow. |

## Hooks

| Hook | Trigger | What it does |
|---|---|---|
| `fields.json` reminder | Any edit or write to `fields.json` | Reminds you to run `date +%s` and update the `modified` timestamp — ACF uses this for field group sync detection |

---

## Requirements

- WordPress 6.0+
- ACF Pro
- PHP 8.0+
- Node.js + pnpm (if using Tailwind or TypeScript)

## License

MIT — [phucbm](https://github.com/phucbm)
