# viola-selfie

Private OpenClaw/Hermes skill for generating Viola-style selfie images through 9Router image models.

- Backend: 9Router `/v1/images/generations`
- Default model: `cx/gpt-5.5-image`
- Fallback models: `cx/gpt-5.4-image`, `cx/gpt-5.3-image`
- Output: local image file, then attach with `MEDIA:<path>`
- No fal.ai dependency
- No public CDN reference required
- No SOUL/persona edit unless `--write-soul` is explicitly used
- No real reference image committed; use private local file path

## Quick install

OpenClaw:

```bash
npx viola-selfie install --target openclaw --reference ./viola-reference.jpg
```

Hermes:

```bash
npx viola-selfie install --target hermes --home ~/.hermes --reference ./viola-reference.jpg
```

Optional capability snippet:

```bash
npx viola-selfie install --target hermes --home ~/.hermes --reference ./viola-reference.jpg --write-soul
```

`--write-soul` only appends a small image capability note. It does not replace the existing persona.

## Reference manager

Store/update private reference image:

```bash
npx viola-selfie set-reference ./viola-reference.jpg --target hermes --home ~/.hermes
```

This copies the image to a private local reference folder, applies `chmod 600`, and updates `.env`.

## Model discovery

```bash
npx viola-selfie models --base-url http://127.0.0.1:20128
```

If `VIOLA_IMAGE_MODEL` is unset, the generator picks the first available model from:

1. `cx/gpt-5.5-image`
2. `cx/gpt-5.4-image`
3. `cx/gpt-5.3-image`

## Manual env

```bash
export NINEROUTER_URL="http://127.0.0.1:20128"
export NINEROUTER_KEY="" # optional if your 9Router has no auth
export VIOLA_IMAGE_MODEL="cx/gpt-5.5-image"
export VIOLA_REFERENCE_IMAGE="/private/local/path/viola-reference.jpg"
export VIOLA_OUTPUT_DIR="$HOME/.openclaw/workspace/generated/viola-selfie"
export VIOLA_SIZE="1024x1024"
```

## Manual install

```bash
mkdir -p ~/.openclaw/skills/viola-selfie
cp -a skill/. ~/.openclaw/skills/viola-selfie/
```

Then add env vars through your OpenClaw config or shell environment.

## Usage idea

Ask the assistant:

- `viola selfie at a cozy cafe`
- `send viola pic wearing black hoodie`
- `show viola in cyberpunk city`

The skill should generate an image, save it locally, then reply with:

```txt
MEDIA:/path/to/generated-image.png
```

## Notes

This repo intentionally does not ship a real Viola reference image. Keep `VIOLA_REFERENCE_IMAGE` as a private local file path when possible.
