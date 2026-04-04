---
name: audit
description: Audit all blocks in the theme against nectarblocks conventions and report issues
---

Audit every block in the active WordPress theme and report any convention violations.

## How to find blocks

Look for `blocks.json` in a `themes/` subfolder. Read the list of block names, then check each block's folder at `themes/{theme}/blocks/{block-name}/`.

## Rules to check

Run all rules against every block. Report each violation with the file path and a short fix description.

---

### Rule 1 — Wrapper attributes

`render.php` must use `get_block_wrapper_attributes()` on the outermost element.

**Fail:**
```php
<div class="my-block">
```

**Pass:**
```php
<div <?php echo get_block_wrapper_attributes(['class' => 'my-block']); ?>>
```

---

### Rule 2 — Admin render file usage

`render-admin.php` should only exist for blocks with interactive or hidden content (sliders, videos, tabs, accordions). Static layout blocks do not need it.

Flag any block that has `render-admin.php` but whose `block.json` title/description suggests it is a simple static block.

---

### Rule 3 — Early return in render.php

If a block has `render-admin.php`, then `render.php` must bail early in the editor using:

```php
if(defined('REST_REQUEST') && REST_REQUEST && file_exists(__DIR__ . '/render-admin.php')){
    include __DIR__ . '/render-admin.php';
    return;
}
```

Flag any block that has `render-admin.php` but `render.php` does not contain this early return.

---

### Rule 4 — Empty state messages

`render-admin.php` must show a placeholder message when there is no content to preview, so the editor does not show a blank space.

Flag any `render-admin.php` that has no visible fallback text or placeholder.

Frontend `render.php` must NOT show empty-state messages — silence is correct on the frontend.

---

### Rule 5 — block.json completeness

`block.json` must have:
- `title` — not empty, not a placeholder like "Block Title" or "My Block"
- `description` — not empty, not a placeholder

Flag any block with missing or placeholder values.

---

### Rule 6 — fields.json modified timestamp

`fields.json` must have a `modified` field with a Unix timestamp. A value of `0` or missing entirely is a violation.

---

## Output format

Group results by block. For each block print:

```
{block-name}
  ✔ Rule 1 — wrapper attributes
  ✗ Rule 3 — missing early return in render.php
  ✔ Rule 5 — block.json complete
```

At the end, print a summary:
- Total blocks checked
- Total violations
- List of blocks with violations
