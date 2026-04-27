# Development Workflow

## 1. Start a new project

```
/wp-blocks-dev:init my-project
```

Claude will ask for your project name, slug, and author, then scaffold the full theme structure. You choose which optional features to include (Tailwind, TypeScript, per-dev CSS).

After init:
```bash
cd themes/my-project
pnpm install
pnpm build
```

Activate the theme in WordPress → Appearance → Themes.

---

## 2. Create a block

```
/wp-blocks-dev:create-block hero-banner
```

Claude asks for the block title, description, and whether it needs frontend JS or an admin preview. It creates all required files and registers the block in `blocks.json`.

Then in WordPress:
1. Go to ACF → Field Groups
2. Find the new block's field group
3. Add your fields
4. Save — ACF writes back to `fields.json` automatically (if `acf-blocks-local-json` mu-plugin is active)
5. Run `date +%s` and update the `modified` field in `fields.json`

---

## 3. Develop the block

Edit `render.php` to output the block's HTML using `get_field()` to access ACF values.

If the block needs frontend JS, edit `view.entry.ts` and run:
```bash
pnpm ts-watch
```

If styling with Tailwind, run:
```bash
pnpm tw-watch
```

Or both at once:
```bash
pnpm dev
```

---

## 4. Add a new JS or CSS asset

Add `wp_enqueue_script()` or `wp_register_script()` calls manually to `inc/enqueue-assets.php`. Always use `filemtime()` as the version and load JS in the footer.

---

## 5. Audit before committing

```
/wp-blocks-dev:audit-blocks
```

Checks all blocks against conventions — wrapper attributes, admin render rules, empty states, `block.json` completeness, `fields.json` modified timestamp. Fix any reported violations before pushing.

---

## 6. Deploy

```bash
pnpm build
```

Commit `assets/css/style.generated.css` — this is the production stylesheet. The server cannot run Node.js so the file must be built locally before deploying.

Do not commit `assets/css/style.*.generated.css` (per-developer files).

---

## Multi-developer CSS setup

Each developer sets their own key in `.env.local` (gitignored):

```
TAILWIND_USER=yourlogin
```

The project lead maps WP user IDs to keys in `functions.php`:

```php
define('DEV_CSS_MAP', [
    1 => 'alice',
    2 => 'bob',
]);
```

Each dev runs `pnpm tw-watch` independently. The `tailwind-theme-loader` mu-plugin automatically serves each logged-in admin their own CSS file.

See `mu-plugins/tailwind-theme-loader.php` for full setup details.
