# Block Conventions

These conventions apply to every block in a wp-blocks-dev project.

## File structure

Every block lives in `blocks/{block-name}/`:

```
blocks/{block-name}/
├── block.json          Required — metadata, ACF mode, viewScript
├── fields.json         Required — ACF field definitions
├── render.php          Required — frontend PHP template
├── render-admin.php    Optional — editor preview (interactive blocks only)
└── view.entry.ts       Optional — frontend JS (compiled to view.js)
```

## block.json

Minimum required fields:

```json
{
  "$schema": "https://schemas.wp.org/trunk/block.json",
  "apiVersion": 3,
  "name": "acf/{block-name}",
  "title": "Human Readable Title",
  "description": "One sentence describing what this block does.",
  "category": "theme",
  "acf": {
    "mode": "preview",
    "renderTemplate": "render.php"
  }
}
```

- `title` and `description` must never be empty or placeholder values
- If the block has frontend JS: add `"viewScript": ["file:./view.js"]`
- If the block depends on a registered library: `"viewScript": ["library-handle", "file:./view.js"]`

## fields.json

ACF field groups exported as JSON. Key rule:

**After every edit, update the `modified` field with the current Unix timestamp:**

```bash
date +%s
```

Paste the output as the `modified` value. ACF uses this for field group sync detection. A stale or zero value causes sync issues.

## render.php

### Wrapper

The outermost element must always use `get_block_wrapper_attributes()`:

```php
<div <?php echo get_block_wrapper_attributes(['class' => 'wp-block-{block-name}']); ?>>
```

This ensures WordPress can inject anchor IDs, additional classes, and other block-level attributes.

### Early return for admin

If the block has a `render-admin.php`, bail early in the editor:

```php
if(is_admin()){
    include __DIR__ . '/render-admin.php';
    return;
}
```

### No empty states on the frontend

If a block has no content, render nothing. Do not show placeholder messages to visitors.

## render-admin.php

### When to create it

Only create `render-admin.php` for blocks where the frontend render cannot be previewed accurately in the editor — typically:

- Video sliders or carousels (autoplay, JS-dependent)
- Animated elements
- Content loaded via AJAX
- Blocks that are visually empty until JS initializes

Static layout blocks (text, images, grids) do not need `render-admin.php`.

### What it must contain

Always show a visible placeholder so the editor does not display a blank block:

```php
<div style="pointer-events:none; padding:2rem; background:#f0f0f0; text-align:center;">
    <p><strong>Block Name</strong> — preview not available in editor</p>
</div>
```

`pointer-events:none` prevents accidental interaction with the admin preview.

## view.entry.ts

Use this file for block-specific frontend JavaScript.

- File name convention: `view.entry.ts` → compiled to `view.js` by tsup
- Scope your selectors to the block wrapper class to avoid conflicts
- Declare the compiled `view.js` in `block.json` viewScript — do not enqueue manually

## Block registration

Blocks are registered by adding the block slug to `blocks.json` at the theme root:

```json
["hero", "about", "contact"]
```

`inc/register-blocks.php` reads this file and calls `register_block_type()` for each entry. Keep the list in alphabetical order.
