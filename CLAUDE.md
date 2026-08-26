# Sparrow Fabrications — Project Guidance

## Version Control

Never commit or push changes without explicitly asking the user first.

## Adding a New Product

### 1. Find the right template

Products are flat `.md` files in the project root. Find an existing product for the same camera type and product type (e.g. shutter release cable adapter, filter adapter, lens cap) and duplicate it. Good sources to duplicate:

- Shutter release cable adapter: `holga-120-v2-adapter.md`
- Filter adapter: `holga-filter.md`
- Lens cap: `holga-120-lens-cap.md`

### 2. Create the product file

Name the file after the product slug, e.g. `holga-digital-adapter.md`. Front matter fields:

```yaml
---
title: "Full descriptive title with camera name | 3D Printed UK"
description: "SEO meta description, ~120 characters."
rating_count: 4   # optional — only add once there are real reviews
---
```

Substitute the camera model name throughout the body copy. The structure follows this pattern:

1. Title (`## `) heading
2. Buy-links block — direct links to this product's own Etsy/eBay UK/eBay US listings (see below)
3. Intro paragraph — what it is, what's shown but not included, link to cable if applicable
4. `### Directions for use` — numbered steps
5. `### Supported Cameras` — tested and compatible models
6. `### Installation Video` — embed from Etsy static hosting (optional, only if video exists)
7. `### Product Images` — gallery include (see below)
8. `### Reviews` — blockquote format (optional, only once reviews exist)
9. `### Also available for [camera]` — links to sibling products

### Buy-links block

Immediately after the `## ` title heading (before the intro/description paragraph), add direct links to this specific product's own listings (not the shop homepage — those live in `purchasing.md`/`index.md`). Leave a blank line both before and after the block so Kramdown renders it correctly:

```html
<div class="shop-links">
  <a href="https://www.etsy.com/uk/listing/<id>/<slug>" class="btn-primary">Buy on Etsy</a>
  <a href="https://www.ebay.co.uk/itm/<item-number>" class="btn-primary">eBay UK</a>
  <a href="https://www.ebay.com/itm/<item-number>" class="btn-primary">eBay US</a>
</div>
```

eBay UK and eBay US share the same item number for a given listing — the US link is just the `.com` version of the same `/itm/<item-number>` path, not a separate lookup.

If a product page covers more than one distinct listing (e.g. separate variants each sold as their own Etsy/eBay listing, like the Holga 120 vs 120S adapter, or the Belair City Slicker vs Jetsetter adapter), place a buy-links block under each variant's own subsection heading instead of once at the top.

eBay's shop, search, and item pages are bot-protected (Akamai) and cannot be reliably fetched or verified programmatically — likewise Etsy's shop/listing pages (DataDome). Don't guess or fabricate listing IDs; get them from the seller directly (e.g. the seller's own dashboard, or a supplied export/mapping) rather than scraping.

### 3. Images

Place full-resolution images in:
```
images/[product-slug]/
```

Place thumbnails in:
```
images/[product-slug]/thumbs/
```

**Thumbnail spec:** resize so the longest axis is ≤ 650px (preserving aspect ratio), then place on a 700×700 white canvas, centred. Save as JPEG quality 85.

Generate thumbnails with Python/Pillow (ImageMagick may not be available):

```python
from PIL import Image
import os

src_dir = "images/[product-slug]"
thumb_dir = os.path.join(src_dir, "thumbs")
os.makedirs(thumb_dir, exist_ok=True)

for fname in os.listdir(src_dir):
    if not fname.endswith(".jpg"):
        continue
    img = Image.open(os.path.join(src_dir, fname)).convert("RGB")
    img.thumbnail((650, 650), Image.LANCZOS)
    canvas = Image.new("RGB", (700, 700), (255, 255, 255))
    canvas.paste(img, ((700 - img.width) // 2, (700 - img.height) // 2))
    canvas.save(os.path.join(thumb_dir, fname), "JPEG", quality=85)
```

**Accessory images** (e.g. `shutter-release-cable.jpg`) are included in the gallery to show items pictured but not included with the product. Copy the shared image into the product's image directory and reference it at the end of the filenames list.

### 4. Gallery include syntax

```liquid
{% include my-gallery.html imagesurl="images/[product-slug]" alt="Descriptive alt text"
   filenames="image-1.jpg,image-2.jpg,shutter-release-cable.jpg" %}
```

### 5. Update products.md

Add a link in the appropriate named section. Sections are grouped by camera brand with nested sub-sections per model. Insert alphabetically by camera name. Example:

```markdown
### Holga Digital Cameras
- [Shutter Release Cable Adapter](holga-digital-adapter.md)
```

If the camera brand already has a section, add the new product as a nested sub-section within it — not as a new top-level section. For example, Holga Digital sits under `### Holga Cameras` as `- Holga Digital Cameras` with indented links beneath it.

Also check the `## Supported Cameras` section further down `products.md`. If this is a new camera model, add it there too (alphabetically, matching the style of existing entries — a linked camera name, plus indented sub-model bullets if applicable). If the camera is already listed there, no change is needed.

### 6. Cross-reference all sibling products

Every product page must have an `### Also available for the [Camera Model]` section at the bottom listing every other product for that camera. When adding a new product:

- Add the new product's page with links to all existing sibling products
- Update every existing sibling product page to add a link back to the new product

If a camera model only has one product, no "Also available" section is needed.
