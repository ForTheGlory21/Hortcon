# Hortcon Handyman Services — Site Guide

A reference for what the site does, what it still needs, and the style rules to follow so new changes look and feel like the existing site.

---

## 1. Overview

| | |
|---|---|
| **Business** | Hortcon Handyman Services — "Property Maintenance Specialist" |
| **Owner named on site** | Steve Barry, Qualified Horticulturalist, phone 0421 848 046 |
| **Live address** | hortcon.com.au (set by the `CNAME` file; hosted on GitHub Pages) |
| **Purpose** | One-page brochure site that shows services and collects quote requests |
| **Tech** | Plain static HTML + CSS + a small inline script. No framework, no build step, no dependencies |
| **Files** | `index.html`, `style.css`, `CNAME`, `README.md`, `imgs/` (service photos + `vines.png`) |
| **Form backend** | Formspree, form ID `meepjlbb` (enquiries are emailed to the address on that Formspree account) |
| **Design approach** | Mobile-first: base CSS targets phones, a single `@media (min-width: 768px)` block scales up |

> `.claude/launch.json` is only for local previewing. Don't upload it to GitHub.

---

## 2. Current features

### Page structure (top to bottom)
1. **Fixed header** (74px tall) — green burger button on the left, brand name + tagline beside it, frosted cream background.
2. **Hanging ivy vine** (`.header-vine-full`) — `imgs/vines.png` repeated horizontally, hanging from the bottom of the header. This is the site's signature visual. It fades out when the menu is open. Its opacity is deliberately set (see the CSS comment), so don't change it without asking.
3. **Slide-down menu** (`.mobile-menu`) — dark green panel with Home / Services / Contact links. Closes when a link is tapped.
4. **Hero** (`#hero`) — Steve's signwritten Hortcon trailer (`imgs/truck.jpg`) with a dark overlay. The photo is positioned differently by screen shape so the trailer signage never sits under the text:
   - **Tall phones:** the photo is oversized (`auto 118%`) and anchored to the bottom, which lifts the signage above the heading.
   - **Wider portrait screens:** `cover`.
   - **Landscape desktops (768px+):** the whole trailer sits on the right, with the text on dark green (`#23442b`) to the left.

   Contents: the eyebrow "Reliable outdoor care", the headline, the credit line "Steve Barry · Qualified Horticulturalist" (`.hero-credit`), and two buttons side by side (`.hero-actions`):
   - **Get a Quote** (`.primary-btn`, light green pill) scrolls to the form
   - **Call Steve 0421 848 046** (`.phone-btn`, white outline pill) is a `tel:` link that dials on phones
5. **Services** (`#services`) — six image-topped cards in a single column:
   - Garden Renovation — a swipeable gallery of three photos (`gardenreno1–3.jpg`) with dots
   - Gutter Cleaning
   - Mowing and Garden Maintenance
   - Stepping Stones (uses `garden-edging.jpg`)
   - Pressure Washing
   - Solar Panel Cleaning
6. **Quote request** (`#contact`) — pale green panel with rounded top corners. Form fields:
   - Name (required)
   - Phone (required)
   - Service dropdown
   - Job description
   - Full-width submit button
7. **Floating "Get a Quote" button** — fixed bottom-right; hides automatically while the contact section is on screen.

### Behaviour (inline `<script>` at the bottom of `index.html`)
- The burger toggles `.open` on the menu and `.menu-open` on `<body>`. Menu links close the menu.
- A scroll listener hides or shows the floating quote button.
- **Card galleries:** any `.card-gallery-wrap` gets dot buttons generated automatically. The dots track the visible photo, and tapping one scrolls to that photo. Swiping uses CSS scroll-snap.
- **Quote form:** sends to Formspree in the background (`fetch` with `Accept: application/json`), so the customer stays on the site.
  - While it sends, the button reads "Sending..." and is disabled.
  - On success, the form clears and a green "Thanks! Steve will be in touch soon." message appears (`.form-status.success`).
  - On failure, a red message asks them to call, with a tap-to-call link (`.form-status.error`).
- Smooth scrolling between sections via `html { scroll-behavior: smooth; }`.

### Decorative extras
- Two blurred, 10%-opacity green "blobs" fixed behind the page.
- Body background gradient from cream (`#f6f4ee`) at the top to strong green (`#2e791a`) at the bottom.

---

## 3. Style guide (keep new work consistent with this)

### Colour palette
There are no CSS variables yet; these hex values are written out directly. Reuse them exactly, and don't introduce new greens.

| Role | Colour | Where it's used |
|---|---|---|
| **Primary dark green** | `#23442b` | Headings, brand name, menu background, floating button, success message text |
| Dark green (hover) | `#2d5636` | Floating button hover |
| Burger green | `#2f5d3a` | Burger button |
| Eyebrow green | `#3d6b47` | Green eyebrow labels on light backgrounds |
| **Accent / CTA green** | `#7bb661` | Primary pill buttons (with `#102010` text), success message border |
| **Cream** | `#f6f4ee` | Page top, header background (at 96% opacity) |
| Pale sage | `#e8efe3` | Contact section background |
| Input border | `#d3dbcf` | Form field borders |
| Body text | `#1f2a1f` | Default text, form text |
| Muted text | `#536053` | Card descriptions |
| Grey text | `#7c7c7c` | Header tagline |
| White | `#ffffff` | Cards, inputs, text on dark backgrounds |
| Error | `#8a2a1c` text / `#fff4f2` background / `#e3b6ad` border | Form error message only |
| Shadow tint | `rgba(30,45,30,0.12)` | Card shadows (always green-tinted, never pure black) |

### Typography
- **Font:** `Arial, Helvetica, sans-serif` everywhere. No web fonts.
- Line height is `1.5` for body text and `1.05–1.1` for headings.
- Sizes in `rem`:
  - Hero headline: `2.1rem` (→ `3rem` on desktop)
  - Section heading: `1.8rem`
  - Card title: `1.28rem`
  - Body text: `~1rem`
  - Brand: `1.05rem` (→ `1.5rem` on desktop)
  - Eyebrow / tagline: `0.82rem`
- **Eyebrow pattern:** every section heading has a small uppercase label above it. Use `<p class="eyebrow">` (add `green` on light backgrounds) followed by an `<h2>`. The label is uppercase with `letter-spacing: 0.12em`.
- Buttons and status messages are bold (`700`).
- Keep headings to headline text only. Put contact details in buttons or links, not inside an `h2`.

### Shape and spacing
- **Rounded, soft shapes.** Nothing has sharp corners.
  - Buttons: fully round pills (`999px`)
  - Cards: `22px`
  - Contact section top corners: `28px`
  - Inputs and status messages: `16px`
  - Burger: `12px`
- Side gutters are `16px` on mobile and `24px` from 768px up.
- Section padding is `34px 16px 100px`. The large bottom padding keeps content clear of the floating button.
- Gaps are `18px` between cards and `10–12px` between buttons and form fields.
- Shadows are soft, large (`0 12px 30px`) and green-tinted.

### Components to reuse
| Need | Use |
|---|---|
| Main call-to-action | `.primary-btn` (light green pill) |
| Secondary action on a dark photo | `.phone-btn` (white outline pill) |
| Row of buttons | `.hero-actions` (flex, wraps on narrow screens) |
| New section | `<section class="…-section" id="…">` + `.section-heading` with eyebrow + `h2` |
| New service | Copy an `<article class="service-card">` block: `img` + `.service-card-content` (`h3` + one-sentence `p`) |
| Several photos on one card | Swap the `img` for `.card-gallery-wrap` > `.card-gallery` (the images) + an empty `.gallery-dots`. The script adds the dots |
| Dark panel / menu | `#23442b` background, white text, `rgba(255,255,255,0.12)` dividers |

### Motion
- Short and subtle: `0.2s–0.3s ease`. Use small lifts on hover (`translateY(-2px)`) and fades to show or hide things.

### Imagery
- Use real photos of Steve's work where possible, cropped with `object-fit: cover` (260px tall on mobile, 340px on desktop).
- **Files must really be JPEG/PNG.** The pressure washing photo was an AVIF file renamed to `.jpg`, which older phones can't display. Convert images properly; don't just rename them.
- Filenames are lowercase and hyphenated (e.g. `pressure-washing.jpg`), with no underscores or spaces. All images live in `imgs/`. (`gardenreno1–3.jpg` and `truck.jpg` came from Steve and were kept as named.)
- Every image has `alt` text describing what's in the photo. Don't copy alt text from another card.
- Aim for about 1400px wide and under 300 KB per photo.

### Voice and copy
- Friendly, plain and practical. Use short sentences and no jargon.
- Each service gets **one sentence** that says what's done and why it helps.
- Write for an Australian audience with Australian spelling ("colour").
- Refer to Steve in the third person in headings ("What Steve can help with"); use "us/our" elsewhere.
- Phone number format: `0421 848 046` for display and `tel:0421848046` for links.

### Code conventions
- Use two-space indentation.
- CSS is grouped under `/* UPPERCASE SECTION */` comments in page order. Add new rules under the matching comment.
- Class names are lowercase and hyphenated. Modifiers are extra classes (`.eyebrow.green`, `.hide`, `.open`, `.success`, `.error`).
- Write mobile styles first. Desktop overrides go in the single `@media (min-width: 768px)` block at the bottom.
- JavaScript stays inline at the end of `index.html`, inside the `DOMContentLoaded` handler, in plain vanilla JS.
- z-index layers: header `1000`, floating button `1000`, menu and vine `999`, blobs `-1`.

---

## 4. Remaining needs (priority order)

### High priority
- **Images are still heavy.** `building-maintenance.jpg`, `garden-edging.jpg` and `solar-cleaning.jpg` are 2.6–3.5 MB each. Compress them and add `loading="lazy"` to the service images. `building-maintenance.jpg` is no longer used, so it can be deleted.
- **No footer, email or service area.** Add a footer with the business name, phone, email, suburbs served and ABN.

### Medium priority
- **The desktop layout is a stretched phone layout.** Use a 2–3 column card grid, a `max-width` container, and inline nav links on wide screens.
- **Accessibility:**
  - The burger needs `aria-label="Menu"` and `aria-expanded`.
  - Form fields have placeholders only, so add `<label>`s.
  - `.primary-btn` needs a hover and focus style.
- **SEO and sharing:** add a `<meta name="description">`, a favicon and Open Graph tags, and mention the area served (this matters for "handyman near me" searches).
- **Wording:**
  - CTAs vary between "Get a Quote", "Get Quote" and "Request a Quote".
  - The solar card copy ("with before and after results…") reads like a note to self rather than customer-facing copy.

### Nice to have
- Store the palette in CSS variables (`:root { --green-dark: #23442b; … }`).
- Before/after gallery, testimonials or Google reviews.
- Close the menu when tapping outside it or pressing Escape.
- Replace the scroll listener with an `IntersectionObserver`.

---

## 5. Checklist for any new change

- [ ] Uses only the existing palette colours (section 3)
- [ ] Arial font, `rem` sizes, eyebrow + `h2` for new sections
- [ ] Rounded corners (pills for buttons, ~22px for cards) and green-tinted soft shadows
- [ ] Built mobile-first; desktop tweaks go in the `768px` media query
- [ ] Checked at phone width (~375px) and on desktop
- [ ] New images are real JPEG/PNG files, compressed, stored in `imgs/`, with hyphenated names and their own accurate `alt` text
- [ ] The vine still displays correctly and doesn't cover anything clickable
- [ ] Copy matches the friendly, one-sentence voice and Australian spelling
- [ ] **New or removed services are updated in both the cards and the form dropdown, with identical names**
- [ ] Nav links updated if a section is added
- [ ] Every changed file is uploaded to GitHub, including renamed or new images
