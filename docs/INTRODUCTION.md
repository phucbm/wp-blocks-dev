# What is wp-blocks-dev?

wp-blocks-dev is a Claude Code plugin for building custom WordPress blocks using ACF (Advanced Custom Fields) and the Gutenberg block editor.

It is not a page builder. It does not add pre-built blocks to your site. It is a **development workflow** — a set of conventions, tools, and AI-powered commands that help developers build custom blocks consistently and efficiently.

## The problem it solves

Building custom Gutenberg blocks with ACF involves a lot of repetitive setup:
- Creating `block.json`, `fields.json`, `render.php` for every block
- Wiring up block registration, asset enqueuing, and ACF field sync
- Remembering which blocks need admin previews, which need frontend JS, how to handle empty states

Without a shared workflow, every developer does this differently. Projects become inconsistent, hard to maintain, and slow to onboard new developers.

## What wp-blocks-dev provides

**A standard block structure** — every block follows the same layout. Any developer familiar with the workflow can read any block immediately.

**AI-powered commands** — scaffold blocks, audit conventions, add assets — all via slash commands that know the full architecture.

**An expert agent** — the `wp-blocks-dev` agent has full context of the workflow built in. No explaining the stack every session.

**Optional build tooling** — Tailwind v4 and TypeScript are supported out of the box but are opt-in. The core workflow is PHP-first.

## What it is not

- Not a replacement for ACF — ACF is required
- Not a visual block builder
- Not opinionated about your CSS methodology (Tailwind is optional)
- Not tied to any specific WordPress theme or starter

## Who it is for

WordPress developers who:
- Build custom sites with unique block designs
- Work in teams and need consistent conventions
- Want AI assistance that understands their specific stack
