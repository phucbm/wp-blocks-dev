# WP Blocks Dev — Architecture Overview

This plugin provides a structured workflow for building custom ACF Gutenberg blocks in WordPress. The core is PHP-first. Tailwind and TypeScript are opt-in.

## How a project is structured

```
themes/{slug}/
├── blocks/              one folder per block, registered via blocks.json
├── blocks.json          list of active block slugs ["hero", "about"]
├── inc/
│   ├── register-blocks.php
│   └── enqueue-assets.php
├── mu-plugins/
│   ├── tailwind-theme-loader.php
│   └── acf-blocks-local-json.php
└── assets/css/ assets/js/
```

## The block lifecycle

1. `/wp-blocks-dev:init` — scaffold the theme once
2. `/wp-blocks-dev:create-block <name>` — scaffold a block (all files + register in blocks.json)
3. Add ACF fields in WP admin → ACF writes back to `fields.json` automatically
4. Edit `render.php` to output HTML using `get_field()`
5. `/wp-blocks-dev:audit-blocks` — verify conventions before committing
6. `pnpm build` — compile Tailwind/TS, commit `style.generated.css`, deploy

## Key rules Claude must always follow

- Every block's outermost element uses `get_block_wrapper_attributes()`
- `render-admin.php` only for interactive/hidden-content blocks (sliders, accordions, JS-dependent)
- If `render-admin.php` exists, `render.php` must bail early with `if(is_admin())`
- Empty states only in `render-admin.php`, never on the frontend
- After any edit to `fields.json`, update the `modified` Unix timestamp via `date +%s`
- Blocks are registered by slug in `blocks.json` — keep alphabetical order

## Knowledge files

Detailed reference for each topic:
- `knowledge/acf-blocks/conventions.md` — full block structure and PHP rules
- `knowledge/acf-blocks/fields.md` — fields.json format and ACF sync
- `knowledge/wordpress/asset-loading.md` — enqueue patterns and mu-plugins
- `knowledge/tailwind/build-pipeline.md` — Tailwind v4 and per-dev CSS
- `knowledge/typescript/tsup-setup.md` — tsup config and entry file convention
