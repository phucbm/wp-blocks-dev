---
name: audit-blocks
description: Fast structural audit of all blocks — checks required files, wrapper attributes, fields.json timestamp, preview.png. No AI, runs instantly via script.
---

Run a fast Level 1 structural audit on all blocks in the active theme.

## What it checks (script only, no AI)

- Required files present: `block.json`, `render.php`
- `fields.json` required only if `render.php` uses `get_field()`
- `preview.png` present (strongly recommended)
- `block.json`: valid JSON, title not empty or placeholder
- `fields.json`: valid JSON, `modified` timestamp not zero or missing
- `render.php`: uses `get_block_wrapper_attributes()`
- `render.php`: if `render-admin.php` exists, `is_admin()` early return is present
- `blocks.json` sync: unregistered folders on disk flagged as warnings

## Run the script

```bash
node $(claude plugin path nectarblocks)/scripts/audit-blocks.js
```

Run from the theme root. Show the full output to the user.

## After results

- For each `✗` issue: fix it before committing
- For each `⚠` warning: address when possible, not blocking
- If all passed: confirm and suggest `/nectarblocks:audit-blocks-deep` for content quality review
