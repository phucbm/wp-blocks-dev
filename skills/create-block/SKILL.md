---
name: create-block
description: Scaffold a new ACF Gutenberg block with block.json, fields.json, and render.php
---

Scaffold a new custom ACF block in the active WordPress theme.

## Steps

### 1. Collect block info

Ask the user for:

- **Block name** — kebab-case slug, e.g. `hero-banner`. This becomes the folder name and block slug.
- **Block title** — human-readable, e.g. `Hero Banner`
- **Block description** — one sentence, e.g. `Full-width hero section with heading and CTA`

Then ask:
- **Needs frontend JS?** — if yes, create `view.entry.ts`
- **Needs admin preview?** — if yes, create `render-admin.php` (for blocks with interactive or hidden content like sliders, videos, or tabs)

### 2. Detect theme path

Look for the active theme by finding `blocks.json` in a `themes/` subfolder. If multiple exist, ask the user which theme to use.

### 3. Check for existing block

If `themes/{theme}/blocks/{block-name}/` already exists, ask the user whether to overwrite or cancel.

### 4. Create block files

**`block.json`**:
```json
{
  "$schema": "https://schemas.wp.org/trunk/block.json",
  "apiVersion": 3,
  "name": "acf/{block-name}",
  "title": "{Block Title}",
  "description": "{Block description}",
  "category": "theme",
  "icon": "block-default",
  "acf": {
    "mode": "preview",
    "renderTemplate": "render.php"
  },
  "supports": {
    "anchor": true
  }
}
```

If frontend JS is needed, add to `block.json`:
```json
"viewScript": ["file:./view.js"]
```

**`fields.json`**:
```json
[
  {
    "key": "group_{block-name-underscored}",
    "title": "{Block Title}",
    "fields": [],
    "location": [
      [
        {
          "param": "block",
          "operator": "==",
          "value": "acf/{block-name}"
        }
      ]
    ],
    "modified": {CURRENT_UNIX_TIMESTAMP}
  }
]
```

Run `date +%s` to get the current Unix timestamp and use it as the `modified` value.

**`render.php`**:
```php
<?php
/**
 * Block: {Block Title}
 */

// Exit if accessed directly
if(!defined('ABSPATH')) exit;

// Bail early in editor if admin render file exists
if(defined('REST_REQUEST') && REST_REQUEST && file_exists(__DIR__ . '/render-admin.php')){
    include __DIR__ . '/render-admin.php';
    return;
}
?>

<div <?php echo get_block_wrapper_attributes(['class' => 'wp-block-{block-name}']); ?>>
    <?php // Block content here ?>
</div>
```

**`render-admin.php`** (only if admin preview requested):
```php
<?php
/**
 * Admin preview: {Block Title}
 * Shown in the editor instead of the full render.
 */
if(!defined('ABSPATH')) exit;
?>

<div style="pointer-events:none; padding:2rem; background:#f0f0f0; text-align:center;">
    <p><strong>{Block Title}</strong> — preview not available in editor</p>
</div>
```

**`view.entry.ts`** (only if JS requested):
```typescript
/**
 * Block: {Block Title}
 * Frontend JS entry — compiled to view.js by tsup
 */

document.querySelectorAll<HTMLElement>('.wp-block-{block-name}').forEach((block) => {
    // Block JS here
});
```

### 5. Register the block

Open `themes/{theme}/blocks.json` and append `"{block-name}"` to the array. Maintain alphabetical order.

### 6. Confirm

Print a summary of files created and remind the user:
- Add ACF fields in the WordPress editor, then re-export `fields.json`
- After editing `fields.json`, run `date +%s` and update the `modified` value
- If JS was added, run `pnpm ts-build` or `pnpm ts-watch`
