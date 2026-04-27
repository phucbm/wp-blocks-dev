# ACF Block Conventions

Every block lives in `blocks/{block-name}/`:

```
blocks/{block-name}/
├── block.json          Required — metadata, ACF mode, viewScript
├── fields.json         Required if any PHP uses get_field() with 1 param
├── render.php          Required — frontend PHP template
├── render-admin.php    Optional — editor preview (interactive blocks only)
└── view.entry.ts       Optional — frontend JS, compiled to view.js by tsup
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
  "icon": "block-default",
  "acf": {
    "mode": "preview",
    "renderTemplate": "render.php"
  },
  "supports": { "anchor": true }
}
```

- `title` and `description` must never be empty or placeholder values
- If the block has frontend JS: add `"viewScript": ["file:./view.js"]`
- If the block depends on a registered library: `"viewScript": ["library-handle", "file:./view.js"]`

## render.php

### Wrapper

The outermost element must always use `get_block_wrapper_attributes()`:

```php
<div <?php echo get_block_wrapper_attributes(['class' => 'wp-block-{block-name}']); ?>>
```

### Early return for admin

If the block has a `render-admin.php`, bail early:

```php
if(is_admin()){
    include __DIR__ . '/render-admin.php';
    return;
}
```

### No empty states on the frontend

If a block has no content, render nothing. Never show placeholder messages to visitors.

## render-admin.php

### When to create it

Only for blocks where the frontend cannot be previewed accurately in the editor:
- Video sliders or carousels (autoplay, JS-dependent)
- Animated or scroll-triggered elements
- Content loaded via AJAX
- Blocks visually empty until JS initializes

Static layout blocks (text, images, grids) do not need `render-admin.php`.

### Required content

Always show a visible placeholder:

```php
<div style="pointer-events:none; padding:2rem; background:#f0f0f0; text-align:center;">
    <p><strong>Block Name</strong> — preview not available in editor</p>
</div>
```

`pointer-events:none` prevents accidental interaction in the editor.

## view.entry.ts

- Compiled to `view.js` by tsup (strips `.entry` from filename)
- Scope selectors to the block wrapper class to avoid conflicts
- Declare compiled `view.js` in `block.json` viewScript — do not enqueue manually

## Block registration

Blocks are registered by adding the slug to `blocks.json` at the theme root:

```json
["about", "hero", "services"]
```

Keep the list in alphabetical order. `inc/register-blocks.php` reads this and calls `register_block_type()` for each entry.
