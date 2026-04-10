---
name: product-card-index-generator
description: Generate or update Anton Oilfield product cards, localized product index entries, and product screenshot assets. Use when adding a new product, syncing a homepage system card with a product page, optimizing product images, or following the existing VGIS card + product index pattern in this repo.
---

# Product Card Index Generator

Use this skill when a product spec exists in `product/*.md` and the goal is to add or update:

- the homepage product card in [index.html](../../../index.html)
- the product modal preview behavior in [index.html](../../../index.html)
- the localized product entry in [product/index.html](../../../product/index.html)
- the product screenshots under `assets/products/<id>/`

This repo's current baseline pattern is the VGIS implementation.

## What to update

1. Read the source markdown for the product in `product/<id>.md`.
2. Update homepage product copy in `i18n.en.systems`, `i18n.zh.systems`, and `i18n.ar.systems` inside [index.html](../../../index.html).
3. Ensure `systemVisuals` in [index.html](../../../index.html) has:
   - `art`
   - `gradient`
   - `href` pointing to `product/index.html?id=<id>`
   - `demoHref` pointing to the real external product site
4. Keep the modal behavior unchanged:
   - modal iframe loads `product/index.html?id=<id>&lang=<lang>&embed=1`
   - CTA text is `Request Demo` or the localized equivalent
   - CTA opens external site in new tab via `demoHref`
5. Add or update the product entry in `productCatalog` inside [product/index.html](../../../product/index.html).

## Product index pattern

Each product entry in `productCatalog` should include:

- `id`
- `name`
- `icon`
- `accentA`
- `accentB`
- `screenshots`
- `tags`
- `locales.en`
- `locales.zh`
- `locales.ar`

Each locale block should include:

- `eyebrow`
- `subtitle`
- `overview`
- `screenshotsLabel`

## Content rules

- Preserve the existing page structure in [product/index.html](../../../product/index.html):
  - top product summary card
  - screenshots section with images only
  - overview section
- Follow the current repo internationalization model:
  - homepage and modal text must use the selected global language
  - product page must render from `lang` query param
- Product page UI labels such as back link, screenshots title, and overview title must also follow the `lang` query param.
- Do not add new sections unless explicitly requested.
- Keep product naming consistent across homepage card, modal title, and product index.
- Prefer concise summary copy on the homepage card and richer summary paragraphs in the product index.

## Screenshot rules

- Homepage modal preview uses the unified product index page, not custom modal body content.
- Product index screenshots may point to `.svg` or `.webp`, depending on the actual asset format in `assets/products/<id>/`.
- The screenshots section is image-only. Do not render per-image captions under the screenshots.
- Screenshot images should fill the full card using cover-style rendering and stay anchored to the left/top so left-side UI is prioritized when cropped.
- In the product detail page, the screenshots area should show 3 images by default on desktop.
- If a product has more than 3 screenshots, enable smooth automatic carousel rotation.
- When carousel mode is active, provide left and right translucent navigation buttons so users can manually control the screenshots.
- On mobile, allow the layout to collapse to a single visible slide while keeping the same carousel behavior.
- If real screenshots are not available, keep the existing placeholder image pattern.

## Screenshot optimization workflow

Use this workflow when the user asks to compress, clean up, crop, or replace product screenshots.

1. Inventory current raster assets first.
   - Check sizes in `assets/products/<id>/` and `assets/hero/`.
   - Prioritize the largest `.png` and `.jpg` files.
2. Prefer high-quality WebP for raster screenshots.
   - Convert `.png` / `.jpg` to `.webp` with `cwebp`.
   - Use a high-quality setting first, for example `-q 92 -m 6 -mt`, then review results.
   - After conversion, update all references in [index.html](../../../index.html) and [product/index.html](../../../product/index.html).
3. Remove old raster originals after references are switched.
   - If `.png` / `.jpg` files are no longer referenced, delete them so repo size and image payload both go down.
4. For hero backgrounds, prefer resizing before over-compressing.
   - If an image is used as a full-screen or cover background, reduce resolution to the practical display size with `ffmpeg` and then encode to WebP.
   - Preserve a visually clean result over chasing the absolute smallest file.
5. For screenshot cleanup, use visual inspection plus targeted crop.
   - Use `view_image` to inspect borders or letterboxing.
   - If screenshots have black bars or export padding, crop only the affected edges with `ffmpeg` and re-encode to WebP.
   - Keep product UI chrome intact; trim borders conservatively and iterate if needed.
6. For screenshot interaction behavior, keep motion intentional and lightweight.
   - Use smooth sliding instead of abrupt replacement when rotating screenshots.
   - If the product has 3 or fewer screenshots, keep the section static.
   - If the product has more than 3 screenshots, auto-rotate and also allow manual previous/next control.
7. Re-check the finished state.
   - Confirm there are no stale `.png` / `.jpg` references left in the repo.
   - Confirm the updated screenshots still render correctly in the product page and homepage modal flow.

## Recommended commands

- Inventory image sizes:
  - `find assets -type f \( -iname '*.png' -o -iname '*.jpg' -o -iname '*.webp' -o -iname '*.svg' \) -exec ls -lh {} \; | sort -k5 -h`
- Convert raster screenshot to WebP:
  - `cwebp -q 92 -m 6 -mt input.png -o output.webp`
- Resize and recompress a large hero image:
  - `ffmpeg -y -i input.webp -vf "scale=2560:-2:flags=lanczos" -c:v libwebp -quality 88 -compression_level 6 -preset picture output.webp`
- Crop residual black borders:
  - `ffmpeg -y -i input.webp -vf "crop=<width>:<height>:<x>:<y>" -c:v libwebp -quality 92 -compression_level 6 -preset picture output.webp`

## Working checklist

- Update localized system descriptions in [index.html](../../../index.html)
- Update or add `systemVisuals.<PRODUCT>` in [index.html](../../../index.html)
- Confirm modal CTA text still uses `Request Demo` / localized equivalent
- Confirm modal CTA opens the external product website, not the local product index
- Add or update `productCatalog.<id>` in [product/index.html](../../../product/index.html)
- Verify `lang` support still works for `en`, `zh`, and `ar`
- Keep the screenshots section image-only
- Keep screenshot images left/top anchored while filling the card
- If screenshots were optimized, confirm references now use the final asset extension such as `.webp`
- If screenshots were cropped, visually verify that black borders or export padding are fully removed
- If screenshots exceed 3 items, verify smooth auto-rotation and left/right manual controls still work

## VGIS reference

When in doubt, mirror the current VGIS implementation:

- homepage system entry for `VGIS` in [index.html](../../../index.html)
- `systemVisuals.VGIS` in [index.html](../../../index.html)
- `productCatalog.vgis` in [product/index.html](../../../product/index.html)
