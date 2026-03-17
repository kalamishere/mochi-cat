# Mochi — The Cozy Cat Game

A browser-based cat petting game powered by the [Pollinations](https://pollinations.ai) API. Pet Mochi to earn Purr Points while she shares philosophical wisdom and generates AI art.

**Play it:** [k4l4m.itch.io/mochi-cat](https://k4l4m.itch.io/mochi-cat)

## How It Works

- **Pet Mochi** by clicking/tapping to earn Purr Points and build streaks
- **AI-generated cat art** updates based on Mochi's mood (via Pollinations image models)
- **AI-generated cat philosophy** — Mochi speaks existential wisdom (via Pollinations text models)
- **Mood system** — mood decays over time; keep petting to keep her happy
- **Model selector** — choose from 20+ text and image models (free and paid)

## Authentication

Uses the [Pollinations BYOP](https://github.com/kalamishere/mochi-auth) (Bring Your Own Pollen) system. Players sign in with their own Pollinations account — the developer pays nothing.

Since itch.io runs games in a sandboxed iframe, the auth flow uses a [GitHub Pages callback page](https://kalamishere.github.io/mochi-auth) where users copy their API key and paste it back into the game.

## Deploying to itch.io

1. Zip `index.html`: `zip mochi-cat.zip index.html`
2. Upload to your itch.io project
3. Tick **"This file will be played in the browser"**
4. Save and publish

## Tech Stack

- Single `index.html` — no build step, no dependencies
- [Pollinations API](https://gen.pollinations.ai/api/docs) for text + image generation
- Google Fonts (Lora + Space Mono)

## Related

- [mochi-auth](https://github.com/kalamishere/mochi-auth) — The callback page + BYOP integration guide

---

*Built by [@kalamishere](https://github.com/kalamishere)*
