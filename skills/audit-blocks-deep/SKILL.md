---
name: audit-blocks-deep
description: Deep AI audit of all blocks — runs structural checks first, then uses AI to review content quality, descriptions, grammar, admin render justification, wrapper usage, and empty-state messages.
---

Run a full Level 2 audit on all blocks. Runs the structural script first, then uses AI to review content quality.

## Step 1 — Run the structural script

```bash
node $(claude plugin path nectarblocks)/scripts/audit-blocks.js
```

Show the output. If there are `✗` issues, tell the user to fix them before proceeding with the deep review. Warnings are fine to continue.

## Step 2 — AI content review

For each block, read the following files and apply the checks below:
- `block.json`
- `fields.json`
- `render.php`
- `render-admin.php` (if exists)

### Content checks

**Block title and description (block.json)**
- Is the title clear and human-readable? Flag anything that looks auto-generated or generic (e.g. "Hero Block", "My Block", "Sample Block")
- Is the description a meaningful sentence? Flag empty, placeholder, or nonsensical descriptions
- Grammar and spelling — flag any errors

**ACF field labels (fields.json)**
- Do field labels make sense for what they describe?
- Are there fields with default labels like "Text", "Image", "Field Label"?
- Flag any field with an empty or placeholder label

**Wrapper attributes**
- Every `render.php` must use `px_get_block_wrapper_attributes()` — never build wrapper classes/attributes manually
- Flag any block that constructs its own `$wrapper_class` string or builds wrapper attributes by hand

**Admin render justification (render-admin.php)**
- Create `render-admin.php` ONLY when the block hides content behind interaction (sliders, sticky scroll, opacity-0 slides, click/scroll-to-reveal)
- Static layout blocks (grids, text, images in normal flow) do NOT need it — `px_get_block_wrapper_attributes()` adds `pointer-events-none` automatically
- Flag blocks with interactive/hidden content that are **missing** `render-admin.php`
- Flag blocks with `render-admin.php` that appear to have a simple static layout (may be unnecessary)
- Flag `render.php` files that still contain inline `is_admin()` conditions (logic should live in `render-admin.php`, not scattered in `render.php`)

**Empty-state messages**
- `render-admin.php` **must** show a descriptive message when required content (fields, posts, images) is empty — so the editor knows what to add
- `render.php` **must NOT** show any empty-state messages — render nothing on the frontend when content is missing

**render.php cleanliness**
- Is there commented-out code that should be cleaned up?
- Are there obvious placeholder comments like `// Block content here` left from scaffolding?

## Output format

Group by block. For each block:

```
{block-name}
  Structural:   ✔ passed
  Title:        ✔ clear
  Description:  ✗ "Sample description for hero block" — generic placeholder, update it
  Fields:       ⚠ field "text_1" has a generic label — rename to something descriptive
  Wrapper:      ✔ uses px_get_block_wrapper_attributes()
  Admin render: ✔ justified (interactive slider)
  Empty states: ⚠ render-admin.php missing empty-state message for images field
```

End with a summary count of findings across all blocks, then ask: **"Fix all issues automatically? (or specify which blocks/rules to fix)"** — only proceed after the user confirms.
