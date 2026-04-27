---
name: audit-blocks
description: Fast structural audit of all blocks — checks required files, wrapper attributes, fields.json timestamp, preview.png. No AI, runs instantly via script.
---

Run a fast Level 1 structural audit on all blocks in the active theme.

## What it checks (script only, no AI)

- Required files present: `block.json`, `render.php`
- `fields.json` required only if any `.php` file in the block calls `get_field()` with 1 param (2-param calls like `get_field('key', $id)` are external lookups and don't count)
- `preview.png` present (strongly recommended)
- `block.json`: valid JSON, title not empty or placeholder
- `fields.json`: valid JSON, `modified` timestamp not zero or missing
- `render.php`: uses `get_block_wrapper_attributes()` (warning if missing, not always required)
- `render.php`: if `render-admin.php` exists, `is_admin()` early return is present
- `blocks.json` sync: unregistered folders on disk flagged as warnings

## Run the script

```bash
node $(claude plugin path wp-blocks-dev)/scripts/audit-blocks.js
```

Run from the theme root. Show the full output to the user.

## After results

Show the script output, then for each issue type present show the relevant fix guide below.

### Fix guides

**missing fields.json — a block PHP file uses get_field() with 1 param**
Go to ACF/SCF → Field Groups, find the group assigned to this block, and press Save Changes. This writes the JSON to the host. Then download the file to your local `blocks/{block-name}/` folder.
Requires the **ACF Blocks Local JSON** mu-plugin — install it if not already active.

**render-admin.php exists but missing is_admin() early return**
Add this at the top of `render.php`, before any other logic:
```php
if(is_admin()){
    include __DIR__ . '/render-admin.php';
    return;
}
```

**missing required file: block.json / render.php**
Scaffold the missing file manually or use `/wp-blocks-dev:create-block` to regenerate.

**fields.json: missing or zero `modified` timestamp**
Run `date +%s` and paste the result as the `modified` value in `fields.json`.

**block.json: title is empty or a placeholder**
Open `block.json` and set a clear, human-readable `title`.

**missing get_block_wrapper_attributes() on wrapper element** *(warning)*
Not all blocks require this. If the block builds its wrapper manually and that is intentional, ignore. Otherwise replace the manual wrapper with `px_get_block_wrapper_attributes()`.

**view.js exists but block.json has no viewScript** *(warning)*
Either add `"viewScript": ["file:./view.js"]` to `block.json`, or delete `view.js` if unused.

**unknown file: {filename}** *(warning)*
Check if the file is needed. If it's a stray screenshot or temp file, delete it.

---

- For each `✗` issue: fix before committing
- For each `⚠` warning: address when possible, not blocking
- If all passed: suggest `/wp-blocks-dev:audit-blocks-deep` for content quality review
