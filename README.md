# Mochi — The Cozy Cat Game

A browser-based cat petting game powered by the [Pollinations](https://pollinations.ai) API. Pet Mochi to earn Purr Points while she shares philosophical wisdom and generates AI art.

**Play it:** [k4l4m.itch.io/mochi-cat](https://k4l4m.itch.io/mochi-cat)

## How It Works

- **Pet Mochi** by clicking/tapping to earn Purr Points and build streaks
- **AI-generated cat art** updates based on Mochi's mood (via Pollinations image models)
- **AI-generated cat philosophy** — Mochi speaks existential wisdom (via Pollinations text models)
- **Mood system** — mood decays over time; keep petting to keep her happy
- **Sleep mode** — Mochi naps after 90s idle; wake her by clicking the overlay
- **Model selector** — choose from 20+ text and image models (free and paid)
- **Journal** — save favourite Mochi quotes and copy them to clipboard

## Authentication

Uses the [Pollinations BYOP](https://github.com/kalamishere/mochi-auth) (Bring Your Own Pollen) system. Players sign in with their own Pollinations account — the developer pays nothing.

Since itch.io runs games in a sandboxed iframe, the auth flow uses a [GitHub Pages callback page](https://kalamishere.github.io/mochi-auth) where users copy their API key and paste it back into the game. Keys must start with `sk_` or `pk_`.

## Deploying to itch.io

```bash
cd /tmp/mochi-work
zip -j ~/Downloads/mochi-cat.zip mochi-cat/index.html
```

Upload to your itch.io project, tick **"This file will be played in the browser"**, then publish.

> Use `-j` (junk paths) so only `index.html` is at the zip root, not inside a subdirectory.

## itch.io Quirks & Known Fixes

itch.io's sandboxed iframe blocks all external image URLs. Any `<img src="https://...">` or CSS `url(https://...)` that isn't loaded from the page's own origin will silently fail.

**Fixes applied:**
- **Sleeping preview image** — replaced `image.pollinations.ai` URL with an inline SVG data URI (`data:image/svg+xml;base64,...`). Zero network requests.
- **Image generation fallback** — replaced `placehold.co` fallback URL with an inline mood-aware SVG. Shows the current mood emoji + "mochi is thinking..." with no network dependency.
- All fallback images are now self-contained and work in any iframe sandbox.

## API Notes (Pollinations)

| Endpoint | Auth required | Notes |
|---|---|---|
| `gen.pollinations.ai/v1/chat/completions` | Any non-empty Bearer token | Works with `gemini-fast`, `openai`, etc. |
| `gen.pollinations.ai/image/{prompt}` | Valid Pollinations key | Returns 401 with fake keys |
| `gen.pollinations.ai/account/balance` | Valid Pollinations key | Returns 401 with fake keys |
| `text.pollinations.ai/{prompt}` | None | Legacy free endpoint, still works |

Chat has a full fallback bank of hardcoded philosophical quotes per mood emoji, so the speech bubble always shows something even if the API is unavailable.

## Layout Notes

- Logged-in: two-column grid (cat panel + right panel), max-width 860px
- Logged-out: single-column, max-width 520px, centred with `margin: 0 auto`
- The `pollenRow` element uses `display:flex` via inline style — always restore it with `style.display = 'flex'`, never `style.display = ''` (which drops the property and falls back to `block`)

## Tech Stack

- Single `index.html` — no build step, no dependencies
- [Pollinations API](https://gen.pollinations.ai) for text + image generation
- Google Fonts (Lora + Space Mono)

## Related

- [mochi-auth](https://github.com/kalamishere/mochi-auth) — The callback page + BYOP integration guide

---

*Built by [@kalamishere](https://github.com/kalamishere)*
