# Mochi — The Cozy Cat Game

A browser-based cat petting game powered by the [Pollinations](https://pollinations.ai) API. Pet Mochi to build your bond while she shares philosophical wisdom and generates AI art.

**Play it:** [k4l4m.itch.io/mochi-cat](https://k4l4m.itch.io/mochi-cat)

## How It Works

- **Pet Mochi** by clicking/tapping to build your bond and earn streaks
- **AI-generated cat art** updates based on Mochi's mood (via Pollinations image models)
- **AI-generated cat philosophy** — Mochi speaks existential wisdom (via Pollinations text models)
- **Bond system** — 10 relationship levels (Stranger → Acquaintance → Companion → ... → One With Mochi), displayed as a header badge
- **Mood system** — mood decays over time; keep petting to keep her happy
- **Sleep mode** — Mochi naps after 90s idle; wake her by clicking the overlay
- **Model selector** — choose from 20+ text and image models (free and paid)
- **Journal** — save favourite Mochi quotes and copy them to clipboard
- **Click juice** — squash & stretch animation, paw particle burst, Web Audio API sounds (5 rotating oscillator-based variants, no audio file dependencies)

## Authentication

Uses the [Pollinations BYOP](https://github.com/kalamishere/mochi-auth) (Bring Your Own Pollen) system. Players sign in with their own Pollinations account — the developer pays nothing.

Since itch.io runs games in a sandboxed iframe, the auth flow uses a [GitHub Pages callback page](https://kalamishere.github.io/mochi-auth) where users copy their API key and paste it back into the game. Keys must start with `sk_` or `pk_`.

### Sign-in page (logged-out state)

When not logged in, a twilight scene SVG is shown: deep purple-to-peach sky, crescent moon, starfield, sparkle accents, and a fully illustrated sleeping orange tabby (tabby stripes, blush, heart nose, whiskers, toe beans, curled tail). The sleep overlay (animated Z Z z + "sign in to wake Mochi" button) sits on top.

Below the cat, two engagement cards are shown (the `#loginPrompt` div):
- Tagline + pill CTA ("Wake Mochi Up") with free-account note
- Bond progression teaser: Stranger → One With Mochi, 10 levels explained

On login, the overlay resets (button text/onclick restored to `wakeMochi()`), `loginPrompt` hides, and the game UI shows. `image.pollinations.ai` is **not** used for the sign-in image — it now requires Turnstile and is unreliable without auth. The SVG is fully self-contained.

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
- **Sleeping preview / sign-in image** — illustrated inline SVG cat (no network request). Shown behind the sleep overlay with animated ZZZ CSS.
- **Image generation fallback** — replaced `placehold.co` fallback URL with an inline mood-aware SVG. Shows the current mood emoji + "mochi is thinking..." with no network dependency.
- **Image generation timeout** — `AbortController` with 20s timeout on `generateCatImage()` ensures `finally` always runs and the loading spinner always clears.
- All fallback images are self-contained and work in any iframe sandbox.

## Header UI

| Element | When shown | Notes |
|---|---|---|
| `📖` Journal button | Logged in | Opens journal overlay |
| `⚙️` Settings button | Logged in | Opens settings; sign-out is here |
| Pollen pill `🌸` | Logged in | Shows pollen balance + "top up ↗" link. `height: 28px`, `background: #fff0f6` |
| Bond badge | Logged in | Shows level title (Lora italic) + progress to next level. `height: 28px`, `background: #ffc8de`. At max level shows `bonded ♾` |

Both pills share `height: 28px; border: 1.5px solid var(--warm-tan); border-radius: 100px` for visual consistency. Pollen pill is lighter (utility); bond badge is deeper pink (status/relationship).

## Bond Level System

10 levels gating AI image backgrounds:

| Level | Title | Points |
|---|---|---|
| 0 | Stranger | 0 |
| 1 | Acquaintance | 50 |
| 2 | Friend | 200 |
| 3 | Close Friend | 500 |
| 4 | Companion | 1,000 |
| 5 | Trusted One | 2,000 |
| 6 | Soulmate | 3,500 |
| 7 | Cat Whisperer | 5,500 |
| 8 | Philosopher King | 8,000 |
| 9 | Cosmic Petter | 10,500 |
| 10 | One With Mochi | 12,000 |

## Layout

- All states: single-column, `max-width: 520px`, centred with `margin: 0 auto`
- `display: flex; flex-direction: column; gap: 28px` on `.main-layout`
- Logged-out: stats grid, mood section, pollen pill, hints all hidden; `.single-col` class added

## API Notes (Pollinations)

| Endpoint | Auth required | Notes |
|---|---|---|
| `gen.pollinations.ai/v1/chat/completions` | Any non-empty Bearer token | Works with `gemini-fast`, `openai`, etc. |
| `gen.pollinations.ai/image/{prompt}` | Valid Pollinations key | Returns 401 with fake keys |
| `gen.pollinations.ai/account/balance` | Valid Pollinations key | Returns 401 with fake keys |
| `text.pollinations.ai/{prompt}` | None | Legacy free endpoint, still works |

Chat has a full fallback bank of hardcoded philosophical quotes per mood emoji, so the speech bubble always shows something even if the API is unavailable.

## Tech Stack

- Single `index.html` — no build step, no dependencies
- [Pollinations API](https://gen.pollinations.ai) for text + image generation
- Google Fonts (Lora + Space Mono)
- Web Audio API for click sounds (oscillator-based, zero audio file dependencies)
- All images inline SVG or Pollinations-generated — no external static assets

## Related

- [mochi-auth](https://github.com/kalamishere/mochi-auth) — The callback page + BYOP integration guide

---

*Built by [@kalamishere](https://github.com/kalamishere)*
