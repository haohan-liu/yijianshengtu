<p align="center">
  <img src="readme-banner.png" alt="OpenListingStudio" width="840" />
</p>

# OpenListingStudio

[![Deploy with Clawnify](https://app.clawnify.com/deploy-button.svg)](https://app.clawnify.com/deploy?repo=clawnify/OpenListingStudio)

**The open-source AI listing studio for e-commerce sellers.** Brand kits, a product library with review ingestion, a review-grounded **Launch** workflow for Amazon-compliant listing copy, and a 12ai-powered Creative Studio for product images. Amazon-first, agent-native, and **BYOK**: it runs on your own model key, with no credits, no seats, and no lock-in.

## What it does

- **Brand kits** — colors, fonts, and tone of voice. Every text generation reads from the product's kit so the copy speaks in your voice.
- **Product library + review ingestion** — products with features, specs, and photos. Pull in real customer reviews three ways:
  - **Paste** — one per line, or a free-text dump the AI splits verbatim (never paraphrased).
  - **CSV upload** — matched by header (`body`/`review`/`text`, optional `rating`, `title`).
  - **Amazon import** — through the local Playwright collector, with paste and CSV/Excel import fallbacks.
- **"Launch listing" workflow** — one click packages the whole run:
  1. **Insights** — pains, desires, objections, and customer vocabulary extracted from the reviews. Every supporting quote is **verified verbatim** against the stored review text server-side; with no reviews the insights fall back to an AI-estimated tier that is clearly labelled and never invents customer voice.
  2. **Listing copy** — title, exactly 5 bullets, description, and backend keywords, validated against Amazon's limits (title ≤ 200 chars, bullets ≤ 250 chars each, description ≤ 2000 chars, search terms ≤ 249 bytes). The editor shows live per-field counters and copy-to-clipboard.
- **Creative Studio** — creates primary and secondary listing images with the two configured 12ai image models, including replication, coherent packages, and style transfer.

## Agent-native

Agents can:

- `POST /api/v1/launches` — run the full launch workflow for a product
- `GET /api/v1/launches/{id}` — read insights and listing copy
- `GET /api/v1/products` — browse the product library

## Bring your own keys

| Variable | Required | Purpose |
|---|---|---|
| `TWELVE_API_KEY` | ✅ | The single 12ai credential used by text, vision, planning, QA and both image models |
| `TWELVEAI_BASE_URL` | optional | 12ai OpenAI-compatible gateway, default `https://new.12ai.org/v1` |
| `AI_TEXT_MODEL` | optional | Shared text/vision model; default `gemini-3.8-flash` |
| `AI_IMAGE_MODEL_PRIMARY` | optional | Primary image model; default `gpt-image-2.5-sunburst` |
| `AI_IMAGE_MODEL_SECONDARY` | optional | Secondary image model; default `gemini-3.1-flash-image` |

## Run it locally

```bash
nvm use 22
pnpm install
cp .dev.vars.example .dev.vars   # add your keys
pnpm dev                          # UI on :5173, API on :8787
```

The stack: React 19 + Tailwind v4 client, Hono API, SQLite database, object storage.

## Deploy with Clawnify

This repo ships a `clawnify.json`, so a Clawnify agent can deploy and operate it end-to-end — reviews in, launch out — from chat.
