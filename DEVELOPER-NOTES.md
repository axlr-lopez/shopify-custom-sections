# Custom Shopify Homepage — Developer Notes

## Overview

Six fully schema-driven Shopify sections for a custom homepage. Every visible
piece of content is editable from the Shopify **Customize → Theme Editor** with
no code changes required.

---

## File Structure

```
assets/
  custom-homepage.css                       ← Shared styles for all sections

sections/
  custom-homepage-hero.liquid               ← Hero banner
  custom-homepage-benefits.liquid           ← Benefit / selling-points bar
  custom-homepage-featured-products.liquid  ← Product showcase
  custom-homepage-collections.liquid        ← Collection showcase
  custom-homepage-reviews.liquid            ← Reviews / testimonials
  custom-homepage-footer.liquid             ← Footer with links & social
```

---

## Installation

1. In your Shopify admin go to **Online Store → Themes → Actions → Edit code**.
2. Upload `assets/custom-homepage.css` to the **Assets** folder.
3. Upload each `.liquid` file to the **Sections** folder.
4. Go to **Customize** (Theme Editor) → open your homepage template.
5. Click **Add section** and add the six custom sections in order:
   - Custom Hero
   - Custom Benefits
   - Custom Featured Products
   - Custom Collections
   - Custom Reviews
   - Custom Footer *(see footer note below)*
6. Populate content and publish.

> **Footer note:** Most themes already render a footer via `theme.liquid`.
> If yours does, either replace it or hide the existing one to avoid a
> duplicate footer. The `custom-homepage-footer.liquid` section can also be
> added to a custom page template instead of the homepage.

---

## Theme Editor — Editable Areas

### Hero (`custom-homepage-hero.liquid`)
| Setting | Description |
|---|---|
| Heading | Main H1 headline |
| Subheading | Descriptive paragraph |
| Primary button text + link | Main CTA |
| Secondary button text + link | Optional second CTA |
| Hero image + alt text | Image picker + accessibility label |
| Layout | Image right / Image left / Full-width centered |
| Background color | Section background |
| Text color | All text in the section |
| Button colors | Button background + text color |
| Padding top / bottom | Spacing sliders (0–160 px) |

### Benefits (`custom-homepage-benefits.liquid`)
Section-level: heading, background/text color, padding.

Each **benefit block** (up to 4, drag to reorder):
- Icon image + alt text
- Title
- Description text
- Optional link (wraps the description)

### Featured Products (`custom-homepage-featured-products.liquid`)
Section-level: title, subtitle, columns per row (2/3/4), View All button,
background/text/button colors, padding.

Each **product card block**:
- **Shopify product picker** — auto-fills image, title, price, and URL
- Manual overrides for image, title, price label, button text, button link

### Collections (`custom-homepage-collections.liquid`)
Section-level: title, subtitle, columns per row (2/3/4), show/hide CTA,
background/text colors, padding.

Each **collection card block**:
- **Shopify collection picker** — auto-fills image, title, description, URL
- Manual overrides for all fields + CTA text

### Reviews (`custom-homepage-reviews.liquid`)
Section-level: title, subtitle, section/card background colors, text color,
padding.

Each **review block** (drag to reorder):
- Star rating (1–5, slider)
- Review text
- Reviewer name
- Optional reviewer photo (with initial fallback)
- Optional product reference text

### Footer (`custom-homepage-footer.liquid`)
Section-level: brand name, about text, contact phone/email/address, copyright
text, background/text colors, padding.

**Link group blocks** (drag to reorder): heading + up to 6 manual label+URL pairs.

**Social link blocks**: platform name + profile URL (displayed as pill buttons
next to contact info).

---

## Technical Notes

- **No third-party apps or page builders** are used.
- All CSS uses a `ch-` prefix (custom-homepage) to avoid conflicts with the
  host theme's styles.
- CSS custom properties (`--ch-hero-pt`, etc.) pass padding values from Liquid
  schema settings into the stylesheet for clean separation.
- Images use Shopify's `image_url` + `image_tag` with `widths` and `sizes`
  attributes for responsive srcset generation.
- The hero image sets `loading: 'eager'` and `fetchpriority: 'high'` as it is
  above the fold; all other images use `loading: 'lazy'`.
- Star ratings in reviews use `aria-label` for screen-reader accessibility.
- `{{ block.shopify_attributes }}` is applied to every block element so the
  Theme Editor can highlight, drag, and select individual blocks.
- Hardcoded content is limited to safe placeholder defaults inside schema
  `"default"` values — all can be overridden from the Theme Editor.

## Responsiveness

| Breakpoint | Behaviour |
|---|---|
| > 900 px (Desktop) | Multi-column grids match the layout reference |
| ≤ 900 px (Tablet) | 2-column products/collections; 2-column footer |
| ≤ 640 px (Mobile) | Hero stacks vertically; products/collections go 1-col; footer goes 1-col |

`prefers-reduced-motion` disables image hover zoom transitions.

---

## Assumptions & Limitations

1. The `custom-homepage-footer.liquid` section is built as a Theme Editor
   section (not a `layout/theme.liquid` override) for maximum Theme Editor
   editability per the spec. In a production build it would typically live in
   the layout file with a footer-specific schema.
2. Link groups in the footer support up to 6 links per group. This is a
   Shopify schema limitation (settings arrays are not iterable); a
   `linklists` approach would allow unlimited links but sacrifices in-editor
   drag-and-drop control of individual links.
3. Product and collection pickers use Shopify's native `product` and
   `collection` setting types, which require the store to have products and
   collections published. Manual override fields are always available as a
   fallback.
4. The CSS is delivered via a single shared asset file. Shopify deduplicates
   `stylesheet_tag` calls by URL, so loading it in each section causes no
   duplicate requests.
