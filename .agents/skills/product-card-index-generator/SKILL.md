---
name: product-card-index-generator
description: Generate or update Anton Oilfield product cards and localized product index entries from product markdown docs. Use when adding a new product, syncing a homepage system card with a product page, or following the existing VGIS card + product index pattern in this repo.
---

# Product Card Index Generator

Use this skill when a product spec exists in `product/*.md` and the goal is to add or update:

- the homepage product card in [index.html](../../../index.html)
- the product modal preview behavior in [index.html](../../../index.html)
- the localized product entry in [product/index.html](../../../product/index.html)

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
- Product index screenshots should point to:
  - `../assets/products/<id>/view-1.svg`
  - `../assets/products/<id>/view-2.svg`
  - `../assets/products/<id>/view-3.svg`
- The screenshots section is image-only. Do not render per-image captions under the screenshots.
- Screenshot images should fill the full card using cover-style rendering and stay anchored to the left/top so left-side UI is prioritized when cropped.
- If real screenshots are not available, keep the existing placeholder image pattern.

## Working checklist

- Update localized system descriptions in [index.html](../../../index.html)
- Update or add `systemVisuals.<PRODUCT>` in [index.html](../../../index.html)
- Confirm modal CTA text still uses `Request Demo` / localized equivalent
- Confirm modal CTA opens the external product website, not the local product index
- Add or update `productCatalog.<id>` in [product/index.html](../../../product/index.html)
- Verify `lang` support still works for `en`, `zh`, and `ar`
- Keep the screenshots section image-only
- Keep screenshot images left/top anchored while filling the card

## VGIS reference

When in doubt, mirror the current VGIS implementation:

- homepage system entry for `VGIS` in [index.html](../../../index.html)
- `systemVisuals.VGIS` in [index.html](../../../index.html)
- `productCatalog.vgis` in [product/index.html](../../../product/index.html)
