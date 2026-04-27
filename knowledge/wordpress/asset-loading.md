# WordPress Asset Loading

## Enqueue pattern

All assets go through `inc/enqueue-assets.php`. Always use `filemtime()` for cache busting and load JS in the footer:

```php
function theme_enqueue_assets(): void {
    wp_enqueue_style(
        'theme-style',
        get_stylesheet_directory_uri() . '/assets/css/style.generated.css',
        [],
        filemtime(get_stylesheet_directory() . '/assets/css/style.generated.css')
    );

    wp_enqueue_script(
        'theme-main',
        get_stylesheet_directory_uri() . '/assets/js/main.js',
        [],
        filemtime(get_stylesheet_directory() . '/assets/js/main.js'),
        true  // load in footer
    );
}
add_action('wp_enqueue_scripts', 'theme_enqueue_assets');
```

## Auto-loaded vs on-demand JS

- **Auto-loaded** — add `wp_enqueue_script()` to `theme_enqueue_assets()` in `inc/enqueue-assets.php`
- **On-demand** — register only via `wp_register_script()`, then declare as a dependency in the block's `block.json` viewScript array

On-demand is preferred for block-specific JS — WordPress only loads it on pages that use the block.

## mu-plugins

Standalone PHP files in `mu-plugins/` — loaded automatically by WordPress, no activation needed.

### tailwind-theme-loader.php

Loads Tailwind CSS on the frontend and inside the Gutenberg editor iframe. Handles per-developer CSS files by reading the logged-in user's ID and mapping it to a CSS key via `DEV_CSS_MAP`.

### acf-blocks-local-json.php

Configures ACF to save field group JSON files per-block into `blocks/{block-name}/fields.json` instead of a central `acf-json/` folder. Required for the per-block `fields.json` workflow.

## Block registration

`inc/register-blocks.php` reads `blocks.json` and registers each block:

```php
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
