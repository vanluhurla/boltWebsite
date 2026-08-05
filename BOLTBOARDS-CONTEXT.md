---
name: boltboards-website
description: Full context for the Bolt Boards static website project at /Users/vanessalucena/Developer/boltWebsite. Use when editing, adding features, fixing bugs, or asking questions about the Bolt Boards website, its pages, drops system, gallery, styling, or content.
---

# Bolt Boards Website

Static HTML/CSS/JS site hosted on GitHub Pages at `boltboards.uk`. No build system — edit files directly.

## Project location

```
/Users/vanessalucena/Developer/boltWebsite/
```

## Pages

| File | Purpose |
|------|---------|
| `index.html` | Landing page — hero, services, CTA |
| `about.html` | About page — story, values, CTA band |
| `gallery.html` | Photo gallery — masonry-style grid |
| `drops.html` | Product drops — listing + detail view (JS-driven, no routing) |
| `contact.html` | Tally embedded contact form (`jakJ64`) |

## Design system

**Fonts** (Google Fonts): `Barlow Condensed` (headings, labels, buttons) + `Barlow` (body text)

**Colours:**
- `--purple`: `rgb(112, 77, 241)` — primary accent, buttons
- `--gold`: `#ffdf0f` — secondary accent, hovers, Drops badge
- `--dark`: `#0a0a0a` — page background
- `--white`: `#ffffff`
- `--off-white`: `#f5f5f3` — light section backgrounds
- Hover purple: `#8a63f5`

**Button classes:**
- `.btn-primary` — purple fill, parallelogram clip-path, `padding: 11px 24px`, `font-size: 13px`, `white-space: nowrap`
- `.btn-outline` — transparent, white border
- `.btn-outline-purple` — outline turns purple on hover
- `.btn-outline-gold` — gold border/text, fills gold on hover (used for Drops button on hero)
- `.nav-cta` — purple nav button, turns gold on hover

**Shared patterns:**
- `clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%)` — parallelogram buttons
- `@keyframes fadeUp` — `opacity:0 translateY(32px)` → visible, used on hero and about image
- Mobile breakpoint: `@media (max-width: 768px)`

## Navigation

All pages share the same nav structure:
```html
Services | About | Gallery | Drops | Contact
```
Active page gets `class="active"` on its link (gold colour).

## Footer

All pages share the same footer: logo + links (Services, About, Gallery, Drops, Contact) + social icons + copyright.

Social icons grouped in one `<li class="footer-social-group">` so they never wrap separately on mobile:
```html
<li class="footer-social-group">
  <a href="https://www.instagram.com/boltboardsuk/" target="_blank"><img src="instagramIcon.png" alt="Instagram" class="footer-icon"></a>
  <a href="https://wa.me/447707774158" target="_blank"><img src="whatsappIcon.png" alt="WhatsApp" class="footer-icon"></a>
</li>
```

WhatsApp number: `447707774158`
Instagram: `https://www.instagram.com/boltboardsuk/`

## Analytics

Google Analytics GA4 tag `G-N54HMG1F9L` added to `<head>` on all pages.

## Favicon & tab title

All pages: `<title>Bolt Boards</title>` and `<link rel="icon" href="Circle Dark.png" type="image/png">`.

---

## Drops system (`drops.html`)

Single-page JS view system — no routing. Two views: `#view-list` (product grid) and `#view-detail` (carousel + info). JS switches between them.

### Product data structure

```js
const drops = [
  {
    id: "D26003",
    slug: "...",
    name: "Black Tolex",
    collection: "Drizzle Collection",
    shortDescription: "...",   // shown on card
    description: "...",        // shown on detail
    price: "£299 + shipping",
    availability: "Available", // "Available" | "Reserved" | "Sold"
    mainImage: "1.jpg",        // used on product card
    images: ["1.jpg", ...],    // carousel images
    specs: ["Overall size: ...", "Usable area: ...", "weight kg (lbs)", ...],
    included: ["Pedalboard", ...],
    month: "june",             // used to group into month sections
    imageFolder: "drop1",      // subfolder under boltWebsite/
    whatsappMessage: "Hi, I'm interested in ..."
  }
];
```

### Adding a new drop

1. Create folder `dropN/` inside `boltWebsite/`
2. Compress images: `sips -Z 1100 -s format jpeg -s formatOptions 65 original.jpeg --out N.jpg`
3. Add product object to `drops` array with correct `month` and `imageFolder`
4. Add a new month section in `#view-list` HTML:
```html
<section class="drops-section" style="padding-top:0;">
  <div class="drops-meta">
    <span class="drop-month-badge">Month YYYY Drop</span>
  </div>
  <div class="products-grid" id="products-grid-MONTH"></div>
</section>
```
5. Add filter in `renderList()`:
```js
document.getElementById('products-grid-MONTH').innerHTML =
  drops.filter(p => p.month === 'MONTH').map((p) => cardHTML(p, drops.indexOf(p))).join('');
```

### Current drops

| ID | Name | Collection | Month | Folder | Availability |
|----|------|-----------|-------|--------|-------------|
| D26003 | Black Tolex | Drizzle Collection | june | drop1 | Available |
| D26004 | Gloss Sparkle Finish | Drizzle Collection | june | drop2 | Available |
| D26005 | Burgundy Tolex | Drizzle Collection | july | drop3 | Available |

### Availability states

- `Available` → purple "Enquire on WhatsApp" + outline "Enquire via Contact Form" buttons
- `Reserved` → gold outline button "Reserved — contact us for similar builds"
- `Sold` → disabled grey button "Sold Out"

### Mobile fixes (already applied in CSS)

- CTA buttons stack vertically (`flex-direction: column`)
- Thumbnails: 4 columns (not 5)
- Dots hidden on mobile
- `.detail-included` constrained with `max-width:100%; box-sizing:border-box`
- Social footer icons grouped in one `li`

---

## Gallery (`gallery.html`)

Images live in root of `boltWebsite/`. Sub-gallery images in `galery/` (note spelling).

Grid: 3-column desktop, 1-column mobile. First item spans 2 columns.
Item backgrounds: `#ffffff` (white).

Current images: `IMG_6417.jpeg`, `openBoard.jpg`, `fromTop.jpg`, `pinkVersion.jpg`, `pedalBoard.jpg`, `galery/1a.jpg` → `galery/5a.jpg`.

---

## Image compression standard

All images compressed with:
```bash
sips -Z 1100 -s format jpeg -s formatOptions 65 input.jpeg --out output.jpg
```
Target: under ~500 KB per image. Gallery images: max 1400px at 75% (older batch).

---

## Key content

**About page body text** (`.about-text-wrap`): Bolt was created for people who live, breathe and enjoy music. Small workshop in Kent. Boards designed with the musician in mind.

**Bullet points (about + index):** "Every board is handcrafted start to finish." / "We work closely with each musician..." / "Built in Kent, shipped to musicians across Europe."

**Service cards (index):** Custom Pedalboards / Expert Wiring / Custom Cables — each with updated descriptions.

**Gallery bottom text:** "We constantly add new photos as builds leave the workshop. Check back often!"
