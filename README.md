# Shopify Custom Homepage Sections

A set of fully theme-editor-configurable Shopify sections for building a custom homepage. Built with Liquid, HTML, and CSS — no third-party apps or page builders required.

---

## Sections Included

| Section | File |
|---|---|
| Hero | `sections/custom-homepage-hero.liquid` |
| Benefits Bar | `sections/custom-homepage-benefits.liquid` |
| Featured Products | `sections/custom-homepage-featured-products.liquid` |
| Collections | `sections/custom-homepage-collections.liquid` |
| Reviews / Testimonials | `sections/custom-homepage-reviews.liquid` |
| Footer | `sections/custom-homepage-footer.liquid` |
| Shared Styles | `assets/custom-homepage.css` |
| Global Settings (merge) | `config/settings_schema_MERGE_THIS.json` |

---

## Installation

### 1. Upload files to your theme

Go to **Online Store → Themes → Actions → Edit code** in your Shopify admin.

- Upload `assets/custom-homepage.css` → **Assets** folder
- Upload each `sections/custom-homepage-*.liquid` file → **Sections** folder

### 2. Add global page width setting (optional but recommended)

Open `config/settings_schema.json` in the code editor and add the object from `config/settings_schema_MERGE_THIS.json` into the array. This adds a **Custom Homepage** group under **Theme Settings** with a page width slider.

### 3. Add sections to your homepage

1. Go to **Online Store → Themes → Customize**
2. Select the **Home page** template
3. Click **Add section** and add each Custom section in order
4. Fill in content, hit **Save**

> **Footer note:** If your theme already renders a footer via `theme.liquid`, remove or hide it to avoid a duplicate footer on the homepage.

---

## Theme Editor Settings

Every visible area is editable without touching code.

### Global (Theme Settings → Custom Homepage)
- **Page Width** — max content width across all sections (900–1600 px)
- **Default Section Background** — fallback background color

### Per-Section (all sections)
- **Show Section** — toggle visibility on/off
- **Text Alignment** — Left / Center / Right
- **Background Color** / **Text Color**
- **Padding Top / Bottom** — spacing sliders

### Hero
- Heading, subheading
- Primary button (text, link, colors)
- Secondary button (text, link) — optional
- Hero image + alt text
- Layout: Image Right / Image Left / Full Width Centered
- **Stack on Mobile** — toggle vertical stacking on small screens

### Benefits
- Section heading
- Up to **4 benefit blocks** (drag to reorder), each with:
  - Icon image + alt text
  - Title, description text, optional link

### Featured Products
- Section title, subtitle
- **Products per row** — 2 / 3 / 4
- View All button (text + link)
- **Product card blocks** (drag to reorder), each with:
  - Shopify product picker (auto-fills image, title, price, URL)
  - Manual overrides for image, title, price label, button text/link
- **Stack on Mobile** toggle

### Collections
- Section title, subtitle
- **Cards per row** — 2 / 3 / 4
- Show/hide CTA label on cards
- **Collection card blocks** (drag to reorder), each with:
  - Shopify collection picker (auto-fills image, title, description, URL)
  - Manual overrides for all fields + CTA text/link
- **Stack on Mobile** toggle

### Reviews
- Section title, subtitle
- Section background, card background, text colors
- **Review blocks** (drag to reorder), each with:
  - Star rating (1–5)
  - Review text
  - Reviewer name + optional photo (shows initial if no photo)
  - Optional product reference text

### Footer
- Brand/store name, about text
- Contact: phone, email, address
- Copyright text (auto-generates if left blank)
- Background + text colors
- **Link group blocks** — heading + up to 6 links each
- **Social link blocks** — platform name + URL (displayed as pill buttons)

---

## Changing the Page Width

**Option 1 — Theme Editor (recommended):**
Go to **Customize → Theme Settings → Custom Homepage** and drag the Page Width slider.

**Option 2 — CSS fallback:**
Edit the default value in `assets/custom-homepage.css`:
```css
.ch-container {
  max-width: var(--ch-page-width, 1200px); /* change 1200px */
}
```

**Option 3 — Per-section override:**
Add `style="--ch-page-width: 1400px;"` to the section element in the `.liquid` file.

---

## Responsive Behavior

| Breakpoint | Layout |
|---|---|
| > 900 px | Full multi-column desktop layout |
| ≤ 900 px | Products and collections collapse to 2 columns; footer to 2 columns |
| ≤ 640 px | Hero stacks vertically; products/collections go 1 column; footer goes 1 column |

Mobile stacking can be disabled per section via the **Stack on Mobile** toggle in the Theme Editor.

---

## File Structure

```
assets/
  custom-homepage.css

sections/
  custom-homepage-hero.liquid
  custom-homepage-benefits.liquid
  custom-homepage-featured-products.liquid
  custom-homepage-collections.liquid
  custom-homepage-reviews.liquid
  custom-homepage-footer.liquid

config/
  settings_schema_MERGE_THIS.json
```

---

## Technical Notes

- All CSS uses a `ch-` prefix to avoid conflicts with the host theme
- CSS custom properties (`--ch-page-width`, `--ch-hero-pt`, etc.) pass schema values into styles cleanly
- Images use Shopify's `image_url` + `image_tag` with `widths` and `sizes` for responsive srcset
- Hero image uses `loading: eager` + `fetchpriority: high` (above the fold); all others use `loading: lazy`
- `{{ block.shopify_attributes }}` on every block enables drag-to-reorder and click-to-select in the Theme Editor
- No hardcoded business content — all text is either schema-driven or a safe placeholder default
- `prefers-reduced-motion` disables image hover zoom transitions

---

## Assumptions & Limitations

- Footer link groups support up to 6 links per group (Shopify schema limitation for manual in-editor link control)
- The footer is built as a section rather than a layout override, for full Theme Editor editability
- Product and collection pickers require published products/collections in the store; manual override fields are always available as a fallback
- The shared CSS file is loaded by each section via `stylesheet_tag` — Shopify deduplicates these by URL so there is no performance impact
