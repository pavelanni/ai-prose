# AI Prose

Source for [aiprose.org](https://aiprose.org), a collection of short stories written by AI. Usually I ask Claude to write a story based on something that happened to me.

The site is built with [Hugo](https://gohugo.io) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Getting started

Requirements: Hugo (extended edition; the site is currently built with 0.169) and Git.

```bash
git clone --recurse-submodules git@github.com:pavelanni/ai-prose.git
cd ai-prose
hugo server -D
```

The dev server runs at <http://localhost:1313> and `-D` includes drafts. If you cloned without `--recurse-submodules`, fetch the theme with `git submodule update --init`.

To build the site into `public/`:

```bash
hugo
```

## Writing a new story

```bash
hugo new posts/story-name/index.md
```

This creates `content/posts/story-name/index.md` from `archetypes/default.md`, with `draft: true`. Put the cover image in the same directory as `index.md`, then remove the `draft` flag (or set it to `false`) when the story is ready to publish.

## Project layout

| Path | Contents |
|------|----------|
| `content/posts/` | Stories, one directory each with `index.md` and cover image |
| `layouts/shortcodes/` | Custom shortcodes (see below) |
| `assets/`, `static/` | CSS overrides and static files |
| `archetypes/` | Template for new posts |
| `themes/PaperMod/` | Theme (git submodule) |
| `hugo.yaml` | Site configuration |
| `public/`, `resources/` | Generated output and cache (git-ignored) |

## Shortcodes

- `{{< audio-player url=... >}}` embeds an audio file (e.g. a narrated version of a story).
- `terminal` displays terminal-style text or code blocks. See `layouts/shortcodes/terminal.html` for usage.

## Narrated audio

`scripts/make_audio.py` turns a post into an MP3 using [Kokoro](https://github.com/hexgrad/kokoro) (local, free text-to-speech) and uploads it to the Cloudflare R2 bucket that serves the audio files.

Prerequisites: [uv](https://docs.astral.sh/uv/), `ffmpeg`, `espeak-ng` (`brew install espeak-ng ffmpeg` on macOS), and `npx wrangler login` for uploads. Python dependencies are installed automatically by `uv` on first run.

```bash
scripts/make_audio.py the-great-cicada --text-only   # preview the cleaned text
scripts/make_audio.py the-great-cicada --no-upload   # synthesize to audio/the-great-cicada.mp3
export R2_BUCKET=<bucket-name>
scripts/make_audio.py the-great-cicada               # synthesize + upload to R2
```

The script prints the `audio-player` shortcode line to paste into the post. It skips the front matter, shortcodes, the `>` prompt blockquote and the DALL-E credit, and reads the title first. Use `--voice` to change the narrator (default `af_heart`) and `--speed` to adjust the pace.

Audio published before November 2026 was generated with [openai.fm](https://www.openai.fm/), including the cat's voice in "Tilda and the Chair", which was created from a text prompt there.

## Deployment

The site is deployed to Cloudflare Pages, built from the `main` branch. Pushing to `origin/main` triggers a deploy.

## Contributing

Feel free to send ideas or whole stories to add to the collection:

- **Ideas:** open an [issue](https://github.com/pavelanni/ai-prose/issues) describing the experience or premise you'd like to see turned into a story.
- **Stories:** open a [pull request](https://github.com/pavelanni/ai-prose/pulls) that adds your story as `content/posts/story-name/index.md` (see [Writing a new story](#writing-a-new-story)), with a cover image if you have one.

Submitted stories are published under the same CC0 license as the rest of the project.

## License

[CC0 1.0 Universal](LICENSE): public domain dedication.
