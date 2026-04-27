---
name: create-block
description: Scaffold a new ACF Gutenberg block with block.json, fields.json, and render.php
---

Scaffold a new custom ACF block by running the create-block script from the plugin.

## Steps

### 1. Collect block info

Ask the user for:

- **Block name** — kebab-case slug, e.g. `hero-banner`
- **Block title** — human-readable, e.g. `Hero Banner`
- **Block description** — one sentence (optional)
- **Needs frontend JS?** — if yes, pass `--js`
- **Needs admin preview?** — if yes, pass `--admin` (for interactive blocks: sliders, videos, tabs)

### 2. Run the script

Run the create-block script from the plugin bin. Use `--js` and `--admin` only if requested:

```bash
node $(claude plugin path wp-blocks-dev)/scripts/create-block.js <name> --title="<title>" --description="<description>" [--js] [--admin]
```

Run from the theme root. The script will:
- Create all required files in `blocks/<name>/`
- Register the block in `blocks.json` (alphabetical order)
- Set the correct Unix timestamp in `fields.json`

### 3. Confirm to the user

Show the script output, then remind:
- Add ACF fields in WordPress admin → Field Groups
- After editing `fields.json`, run `date +%s` and update the `modified` value
- If `--js` was used, run `pnpm ts-build` or `pnpm ts-watch`
