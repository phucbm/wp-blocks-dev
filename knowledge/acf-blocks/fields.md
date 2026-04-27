# ACF fields.json

ACF field groups are exported as JSON and stored per-block in `fields.json`.

## When fields.json is required

Only required if any `.php` file in the block calls `get_field()` with **1 parameter**:

```php
get_field('key')           // 1 param = block-owned field = needs fields.json
get_field('key', $post_id) // 2 params = external field = no fields.json needed
```

## Format

```json
[
  {
    "key": "group_{block_name}",
    "title": "Block Title",
    "fields": [],
    "location": [[{ "param": "block", "operator": "==", "value": "acf/{block-name}" }]],
    "modified": 1700000000
  }
]
```

## Modified timestamp — critical rule

After **every** edit to `fields.json`, update the `modified` field with the current Unix timestamp:

```bash
date +%s
```

Paste the output as the `modified` value. ACF uses this for field group sync detection. A stale or zero value causes sync issues where ACF ignores local JSON.

A hook in this plugin automatically reminds you when `fields.json` is edited.

## Adding fields via WordPress admin

1. Go to ACF → Field Groups
2. Find the group assigned to this block
3. Add or edit fields, then Save
4. If the `acf-blocks-local-json` mu-plugin is active, ACF writes back to `fields.json` automatically
5. Run `date +%s` and update `modified`

## Safe fields to edit in fields.json directly

- `label`, `instructions`, `required`, `default_value`, `placeholder` — safe to edit
- `name` — **never change** — maps to saved database content, changing it orphans existing data
- `key` — **never change** — ACF uses it as the field identifier
