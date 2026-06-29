---
name: viola-selfie
description: Generate Viola-style selfie images through 9Router image models. Use only for explicit Viola selfie/pic/photo requests.
---

# Viola Selfie

Generate a Viola-style selfie image through 9Router `/v1/images/generations`, save it as a local file, and attach it with `MEDIA:<path>`.

## Use when

User explicitly asks for Viola visual output, e.g.

- "viola selfie"
- "send viola pic"
- "show viola at a cafe"
- "viola wearing a hoodie"
- "pic of viola in cyberpunk city"

Do not use for generic image requests unless user mentions Viola.

## Required env

- `NINEROUTER_URL` — e.g. `http://127.0.0.1:20128`
- `NINEROUTER_KEY` — optional if auth disabled
- `VIOLA_REFERENCE_IMAGE` — private URL, data URL, or local path for Viola reference image. Local files are converted to data URLs by the script.

## Optional env

- `VIOLA_IMAGE_MODEL` — default auto-discovered from `cx/gpt-5.5-image`, `cx/gpt-5.4-image`, `cx/gpt-5.3-image`
- `VIOLA_MODEL_FALLBACKS` — comma-separated fallback model list
- `VIOLA_OUTPUT_DIR` — default `~/.openclaw/workspace/generated/viola-selfie`
- `VIOLA_SIZE` — default `1024x1024`

## Workflow

1. Extract the requested scene/outfit/location from user text.
2. Pick mode:
   - `mirror` for outfit, wearing, clothes, full-body, mirror.
   - `direct` for cafe, city, beach, close-up, portrait, smile, location.
3. Never paste or reveal `VIOLA_REFERENCE_IMAGE` if it contains a private URL or local path.
4. Run:

```bash
node ~/.openclaw/skills/viola-selfie/scripts/generate-viola.js --context "<scene>" --mode auto
```

If installed with the CLI, this also works:

```bash
viola-selfie generate --context "<scene>" --mode auto
```

5. The script prints `MEDIA:<path>` on success.
6. Reply with that `MEDIA:<path>` line, plus at most one short caption.

## Prompt rules

Keep Viola consistent with the reference image. Avoid claiming the image is a real photo. Do not include private reference URLs in chat.
