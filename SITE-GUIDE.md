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
| **Look** | "Night Forest": dark forest green, lime-green highlights, Outfit font (chosen Oct 2026 from five options) |
| **Design approach** | Mobile-first: base CSS targets phones; `768px` and `1000px` media queries scale up |

> `.claude/launch.json` is only for local previewing. Don't upload it to GitHub.

---

## 2. Current features

### Page structure (top to bottom)
1. **Fixed header** (74px tall) — green burger button on the left, brand name + tagline beside it, frosted dark green background.
2. **Hanging ivy vine** (`.header-vine-full`) — `imgs/vines.png` repeated horizontally, hanging from the bottom of the header. This is the site's signature visual. It fades out when the menu is open. Its opacity is deliberately set (see the CSS comment), so don't change it without asking.
3. **Slide-down menu** (`.mobile-menu`) — dark green panel with Home / Services / Contact links. Closes when a link is tapped.
4. **Hero** (`#hero`) — Steve's signwritten Hortcon trailer (`imgs/truck.jpg`) with a dark overlay. The photo is positioned differently by screen shape so the trailer signage never sits under the text:
   - **Tall phones:** the photo is oversized (`auto 118%`) and anchored to the bottom, which lifts the signage above the heading.
   - **Tablets and wider portrait screens:** `cover`.
   - **Desktop (1000px+):** the hero splits in two. Text sits on a dark green glow on the left, and the trailer is a framed, rounded photo on the right (`.hero-photo`, an `<img>` that's hidden on smaller screens).

   Contents: the eyebrow "Reliable outdoor care", the headline, the credit line "Steve Barry · Qualified Horticulturalist" (`.hero-credit`), and two buttons side by side (`.hero-actions`):
   - **Get a Quote** (`.primary-btn`, lime pill with dark text) scrolls to the form
   - **Call Steve 0421 848 046** (`.phone-btn`, white outline pill) is a `tel:` link that dials on phones
5. **Services** (`#services`) — six image-topped cards: one column on phones, two from 768px, three from 1000px:
   - Garden Renovation — a swipeable gallery of three photos (`gardenreno1–3.jpg`) with dots
   - Gutter Cleaning
   - Mowing and Garden Maintenance
   - Stepping Stones (uses `garden-edging.jpg`)
   - Pressure Washing — a gallery of `pressurewash1.jpg`, `pressurewash2.jpg` (Steve's own jobs), then the stock `pressure-washing.jpg`
   - Solar Panel Cleaning
6. **Quote request** (`#contact`) — slightly lighter dark-green panel with rounded top corners, centred at 720px max on wider screens. Form fields:
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
- Two blurred, 8%-opacity "blobs" (lime and green) fixed behind the page.
- Body background gradient from `#0f1a12` at the top to `#0a120c` at the bottom (near-black forest green).

---

### Layout by screen size
- **Phone:** single column of service cards; the trailer photo fills the hero behind the text.
- **768px+:** two-column card grid, centred with a 1200px max width.
- **1000px+:** three-column card grid. The hero splits into text on the left and the trailer as a framed photo on the right (`.hero-photo`).

---

## 3. Style guide (keep new work consistent with this)

### Colour palette
All colours, fonts and corner sizes are CSS variables in the `:root` block at the top of `style.css`. Always use the variable (e.g. `var(--accent)`), never a raw hex value, so the whole look can be changed in one place.

This is a **dark** site: light text on dark backgrounds. New sections should sit on `--page-top`, `--surface` or `--panel`, never on white.

| Variable | Value | Where it's used |
|---|---|---|
| `--page-top` / `--page-bottom` | `#0f1a12` / `#0a120c` | Page background gradient |
| `--surface` | `#17241a` | Service cards, form inputs, success message |
| `--panel` | `#132017` | Quote section background |
| `--primary` / `--primary-hover` | `#1d3323` / `#26422d` | Menu background, floating quote button |
| `--burger` | `#26422d` | Burger button |
| **`--accent`** | `#a8d65c` (lime) | Main buttons (with `--accent-text` `#0f1a12`), success border |
| `--eyebrow` | `#a8d65c` | Small uppercase section labels |
| `--heading-color` | `#f2f7ef` | Headings, brand name |
| `--text` | `#e8efe6` | Body text, input text |
| `--text-muted` | `#a9b8a8` | Card descriptions |
| `--text-faint` | `#8fa08e` | Header tagline |
| `--border` | `#2a3d2e` | Input borders |
| `--header-bg` | `rgba(15,26,18,0.92)` | Frosted header |
| `--hero-shade` | `8, 14, 10` (RGB triplet) | Hero overlay and button shadows, used as `rgba(var(--hero-shade), 0.5)` |
| `--hero-bg` | radial dark green glow | Desktop hero background |
| `--shadow` | `rgba(0,0,0,0.4)` | Card shadows |
| Error (not a variable) | `#8a2a1c` text / `#fff4f2` background | Form error message only, so it stands out |

### Typography
- **Font:** **Outfit** (Google Fonts, weights 400/600/700, loaded by the `@import` at the top of `style.css`), falling back to Arial. Set through `--font-body` and `--font-heading`.
- Line height is `1.5` for body text and `1.05–1.1` for headings.
- Sizes in `rem`:
  - Hero headline: `2.1rem` (→ `3rem` at 768px, `3.4rem` at 1000px)
  - Section heading: `1.8rem`
  - Card title: `1.28rem`
  - Body text: `~1rem`
  - Brand: `1.05rem` (→ `1.5rem` on desktop)
  - Eyebrow / tagline: `0.82rem`
- **Eyebrow pattern:** every section heading has a small uppercase label above it. Use `<p class="eyebrow green">` (lime) followed by an `<h2>`; plain `.eyebrow` (no `green`) inherits the text colour. The label is uppercase with `letter-spacing: 0.12em`.
- Buttons and status messages are bold (`700`).
- Keep headings to headline text only. Put contact details in buttons or links, not inside an `h2`.

### Shape and spacing
- **Rounded, soft shapes.** Nothing has sharp corners.
  - Buttons: fully round pills (`--radius-btn`, `999px`)
  - Cards and the desktop hero photo: `--radius-card`, `20px`
  - Contact section top corners: `--radius-panel`, `28px`
  - Inputs and status messages: `--radius-input`, `14px`
  - Burger: `12px`
- Side gutters are `16px` on mobile and `24px` from 768px up.
- Section padding is `34px 16px 100px`. The large bottom padding keeps content clear of the floating button.
- Gaps are `18px` between cards (`24px` in the grid) and `10–12px` between buttons and form fields.
- Wider screens centre content at a 1200px max width (720px for the quote form).
- Shadows are soft and large (`0 12px 30px var(--shadow)`); on the dark background they read as depth rather than outlines.

### Components to reuse
| Need | Use |
|---|---|
| Main call-to-action | `.primary-btn` (lime pill) |
| Secondary action on a dark photo | `.phone-btn` (white outline pill) |
| Row of buttons | `.hero-actions` (flex, wraps on narrow screens) |
| New section | `<section class="…-section" id="…">` + `.section-heading` with eyebrow + `h2` |
| New service | Copy an `<article class="service-card">` block: `img` + `.service-card-content` (`h3` + one-sentence `p`) |
| Several photos on one card | Swap the `img` for `.card-gallery-wrap` > `.card-gallery` (the images) + an empty `.gallery-dots`. The script adds the dots |
| Dark panel / menu | `var(--primary)` background, white text, `rgba(255,255,255,0.12)` dividers |

### Motion
- Short and subtle: `0.2s–0.3s ease`. Use small lifts on hover (`translateY(-2px)`) and fades to show or hide things.

### Imagery
- Use real photos of Steve's work where possible, cropped with `object-fit: cover` (260px tall on phones, 340px at 768px, 300px in the 3-column grid).
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
- Write mobile styles first. Overrides go in the `@media (min-width: 768px)` and `@media (min-width: 1000px)` blocks at the bottom.
- JavaScript stays inline at the end of `index.html`, inside the `DOMContentLoaded` handler, in plain vanilla JS.
- z-index layers: header `1000`, floating button `1000`, menu and vine `999`, blobs `-1`.

---

## 4. Remaining needs (priority order)

### High priority
- **Images are still heavy.** `building-maintenance.jpg`, `garden-edging.jpg` and `solar-cleaning.jpg` are 2.6–3.5 MB each. Compress them and add `loading="lazy"` to the service images. `building-maintenance.jpg` is no longer used, so it can be deleted.
- **No footer, email or service area.** Add a footer with the business name, phone, email, suburbs served and ABN.

### Medium priority
- **Desktop nav:** wide screens still use the burger menu; inline nav links in the header would suit desktop better.
- **Accessibility:**
  - The burger needs `aria-label="Menu"` and `aria-expanded`.
  - Form fields have placeholders only, so add `<label>`s.
  - `.primary-btn` needs a hover and focus style.
- **SEO and sharing:** add a `<meta name="description">`, a favicon and Open Graph tags, and mention the area served (this matters for "handyman near me" searches).
- **Wording:**
  - CTAs vary between "Get a Quote", "Get Quote" and "Request a Quote".
  - The solar card copy ("with before and after results…") reads like a note to self rather than customer-facing copy.

### Nice to have
- Before/after gallery, testimonials or Google reviews.
- Close the menu when tapping outside it or pressing Escape.
- Replace the scroll listener with an `IntersectionObserver`.

---

## 5. Checklist for any new change

- [ ] Colours come from the `:root` variables, with light text on dark backgrounds (section 3)
- [ ] Outfit font via the variables, `rem` sizes, lime eyebrow + `h2` for new sections
- [ ] Rounded corners from the `--radius-*` variables and soft shadows
- [ ] Built mobile-first; wider-screen tweaks go in the `768px` / `1000px` media queries
- [ ] Checked at phone width (~375px), tablet (~768px) and desktop (~1440px)
- [ ] New images are real JPEG/PNG files, compressed, stored in `imgs/`, with hyphenated names and their own accurate `alt` text
- [ ] The vine still displays correctly and doesn't cover anything clickable
- [ ] Copy matches the friendly, one-sentence voice and Australian spelling
- [ ] **New or removed services are updated in both the cards and the form dropdown, with identical names**
- [ ] Nav links updated if a section is added
- [ ] Every changed file is uploaded to GitHub, including renamed or new images
