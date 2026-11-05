# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is a Hugo static site generator project for AI Prose (aiprose.org), a collection of AI-generated stories. The site uses the PaperMod theme and contains blog posts with stories written by AI based on personal experiences.

## Commands

### Development server

```bash
hugo server -D
```

Starts the local development server with drafts enabled at http://localhost:1313

### Build site

```bash
hugo
```

Builds the static site to the `public/` directory

### Create new post

```bash
hugo new posts/post-name/index.md
```

Creates a new post in the content/posts directory

## Architecture

The site structure follows Hugo conventions:
- `content/posts/` - Blog posts, each in its own directory with an index.md file
- `layouts/shortcodes/` - Custom shortcodes including audio-player.html and terminal.html
- `static/` - Static assets
- `public/` - Generated site output
- Theme: PaperMod (installed as git submodule in themes/PaperMod)

## Custom features

- Audio player shortcode: `{{< audio-player url=... >}}` for embedding audio files
- Terminal shortcode for code display
- Stories often include cover images stored in the post directory