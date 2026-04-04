---
name: enqueue
description: Add a new JS or CSS asset to the theme enqueue system correctly
---

Add a new JavaScript or CSS asset to the theme's enqueue system.

## Locate the enqueue file

Find `inc/enqueue-assets.php` in the active theme.

## Ask the user

- **Asset type** — JS or CSS?
- **Handle** — the unique WP handle, e.g. `my-slider`
- **File path** — relative to theme root, e.g. `/assets/js/my-slider.js`
- **Dependencies** — array of handles this asset depends on, e.g. `['jquery']`
- **Load behavior** — choose one:
  - **Auto-load** — enqueued on every page
  - **On-demand** — registered only; blocks or other code enqueue it when needed

## Two arrays — use the right one

**Auto-load** → add to the auto-enqueue section:
```php
function theme_enqueue_assets(): void {
    wp_enqueue_script(
        'my-slider',
        get_stylesheet_directory_uri() . '/assets/js/my-slider.js',
        [],
        filemtime(get_stylesheet_directory() . '/assets/js/my-slider.js'),
        true // footer
    );
}
add_action('wp_enqueue_scripts', 'theme_enqueue_assets');
```

**On-demand** → register only, do not enqueue:
```php
wp_register_script(
    'my-slider',
    get_stylesheet_directory_uri() . '/assets/js/my-slider.js',
    [],
    filemtime(get_stylesheet_directory() . '/assets/js/my-slider.js'),
    true // footer
);
```

Then in any block's `block.json`, declare it as a dependency:
```json
"viewScript": ["my-slider", "file:./view.js"]
```

## Rules

- Always use `filemtime()` as the version — never a hardcoded string
- Always load JS in the footer (`true` as last argument)
- Never use `wp_enqueue_script()` for on-demand assets — only `wp_register_script()`
- CSS assets follow the same pattern using `wp_enqueue_style()` / `wp_register_style()`

## Confirm

Show the user the exact code added and which file was modified.
