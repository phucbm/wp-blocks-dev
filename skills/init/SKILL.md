---
name: init
description: Initialize a new WordPress theme with the wp-blocks-dev block development workflow
---

Initialize a new WordPress theme project using the wp-blocks-dev workflow.

## Steps

### 1. Collect project info

Ask the user for the following. Collect all answers before proceeding:

- **Project name** — human-readable, e.g. `My Client Site`
- **Theme slug** — kebab-case, used as folder name and text domain, e.g. `my-client-site`. Suggest a slug derived from the project name.
- **Author** — developer or agency name, e.g. `phucbm`

### 2. Confirm optional features

Ask which optional features to include. Default is yes for all:

- **Tailwind v4** — CSS build pipeline with per-developer CSS support
- **TypeScript + tsup** — JS bundler for entry files and npm packages
- **Per-developer CSS** — multi-dev Tailwind setup via `.env.local` and `DEV_CSS_MAP`

### 3. Check for existing files

Before creating any file, check if it already exists. If it does, ask the user whether to overwrite or skip. Never silently overwrite.

### 4. Scaffold the theme

Create the following structure under `themes/{slug}/`:

```
themes/{slug}/
├── style.css
├── index.php
├── functions.php
├── blocks.json
├── QUICKSTART.md
├── blocks/
├── inc/
│   ├── register-blocks.php
│   └── enqueue-assets.php
├── helpers/
└── assets/
    ├── js/
    └── css/
```

**`style.css`** — WordPress theme header:
```css
/*
Theme Name: {Project Name}
Author: {Author}
Text Domain: {slug}
Version: 1.0.0
*/
```

**`functions.php`** — minimal, standard WP hooks only:
```php
<?php
if(!defined('PX_PROJECT_NAME')){
    define('PX_PROJECT_NAME', '{Project Name}');
}

require_once get_stylesheet_directory() . '/inc/register-blocks.php';
require_once get_stylesheet_directory() . '/inc/enqueue-assets.php';

// Dev CSS mapping (optional) — used by Tailwind Theme Loader mu-plugin
// define('DEV_CSS_MAP', [
//     1 => 'yourlogin',
// ]);
```

**`blocks.json`** — empty registry:
```json
[]
```

**`index.php`** — silent fallback:
```php
<?php // Silence is golden.
```

**`inc/register-blocks.php`** — reads blocks.json and registers each block via ACF:
```php
<?php
if(!defined('ABSPATH')) exit;

function register_custom_blocks(): void {
    $json = file_get_contents(get_stylesheet_directory() . '/blocks.json');
    $blocks = json_decode($json, true) ?? [];
    foreach($blocks as $block_name){
        $block_path = get_stylesheet_directory() . '/blocks/' . $block_name;
        if(is_dir($block_path)){
            register_block_type($block_path);
        }
    }
}
add_action('init', 'register_custom_blocks');
```

**`inc/enqueue-assets.php`** — standard WP enqueue, no custom abstractions:
```php
<?php
if(!defined('ABSPATH')) exit;

function theme_enqueue_assets(): void {
    // Add your wp_enqueue_script() and wp_enqueue_style() calls here.
    // See QUICKSTART.md for the recommended pattern.
}
add_action('wp_enqueue_scripts', 'theme_enqueue_assets');
```

**`QUICKSTART.md`** — point to the wp-blocks-dev plugin docs:
```markdown
# Quick Start

This theme uses the wp-blocks-dev workflow.
See the full guide: `/wp-blocks-dev:guide`

## Commands
- `/wp-blocks-dev:create-block <name>` — scaffold a new block
- `/wp-blocks-dev:audit-blocks` — audit all blocks against conventions
```

### 5. If Tailwind selected

Run these commands to copy build scripts from the plugin templates:

```bash
mkdir -p themes/{slug}/scripts themes/{slug}/assets/css
cp $(claude plugin path wp-blocks-dev)/templates/tailwind.js themes/{slug}/scripts/tailwind.js
cp $(claude plugin path wp-blocks-dev)/templates/create-block.js themes/{slug}/scripts/create-block.js
```

Then create:
- `assets/css/index.css` — Tailwind v4 entry with `@import "tailwindcss"`
- `.env.local.example` — with content: `TAILWIND_USER=yourlogin`
- Add to `.gitignore`: `.env.local` and `assets/css/style.*.generated.css`

Remind the user to copy `tailwind-theme-loader.php` into `mu-plugins/`.

### 6. If TypeScript selected

Add to `themes/{slug}/`:
- `tsup.config.ts` — entry file glob: `**/*.entry.ts`
- `tsconfig.json` — standard config
- `assets/js/index.entry.ts` — empty global JS entry

Add to `package.json` scripts:
```json
{
  "scripts": {
    "ts-build": "NODE_ENV=production tsup",
    "ts-watch": "NODE_ENV=development tsup --watch",
    "tw-build": "node ./scripts/tailwind.js",
    "tw-watch": "node ./scripts/tailwind.js --watch",
    "dev":   "pnpm ts-watch & pnpm tw-watch",
    "build": "pnpm ts-build && pnpm tw-build",
    "cb":    "node scripts/create-block.js"
  }
}
```

### 7. Confirm completion

Print a summary of what was created, what was skipped, and the next steps:
- Activate the theme in WordPress
- Run `pnpm install && pnpm build` if Tailwind or TS was selected
- Use `/wp-blocks-dev:create-block` to add the first block
