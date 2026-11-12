# Hortcon Handyman Services — Site Guide

A reference for what the site does, what it still needs, and the style rules to follow so new changes look and feel like the existing site.

---

## 1. Overview

| | |
|---|---|
| **Business** | Hortcon Handyman Services — "Property Maintenance Specialist" |
| **Owner named on site** | Steve Barry, Qualified Horticulturalist, phone 0421 848 046 |
| **Live address** | hortcon.com.au (set by the `CNAME` file; hosted on GitHub Pages) |
| **Purpose** | One-page site that shows Steve's work and gets people to call or ask for a quote |
| **Audience** | Locals, mostly on phones |
| **Tech** | Plain static HTML + CSS + a small inline script. No framework, no build step, no dependencies |
| **Files** | `index.html`, `style.css`, `CNAME`, `README.md`, `imgs/` |
| **Form backend** | Formspree, form ID `meepjlbb` (enquiries are emailed to the address on that Formspree account) |
| **Look** | "The Trailer" (Nov 2026): black, emerald and silver taken from Steve's trailer signwriting, brightened with cream and lawn-green sections, ivy and motion |
| **Design approach** | Mobile-first: base CSS targets phones; `768px` and `1000px` media queries scale up |

> **Never submit the quote form to test it.** It emails Steve as if it were a real customer.
>
> `.claude/` and `mockups/` are only for working on the site. Don't upload them to GitHub.

---

## 2. Current features

### Page structure (top to bottom)
1. **Header** (`.site-header`, sticky, 60px) — the HortCon logo on a slanted silver plate (`.logo`, built from text, no image) and a green call button (`.header-call`). From 768px the button also shows the phone number.
2. **Hero** (`.hero`)
   - **Slideshow** (`.hero-photo`): three photos fade into each other with a slow zoom. Each `<i>` inside is one slide; the photos are set in `style.css`.
   - **Ivy** (`.hero::before`): `imgs/vines.png` hangs under the header and sways.
   - **Seal** (`.seal`): the lime "Qualified Horticulturalist · Steve Barry" badge. The text ring spins.
   - **Headline**: "We go the extra yard." (the slogan on the trailer) in script, written on when the page loads, with a hand-drawn yellow underline.
   - **Buttons** (`.hero-actions`): "Call Steve 0421 848 046" (`tel:` link) and "Get a quote" (scrolls to the form).
3. **Ticker** (`.ticker`) — slanted green band scrolling the services listed on the trailer. The two `<p>` lines must stay identical or the loop shows a gap.
4. **Trust strip** (`.trust`) — three short reasons to pick Steve.
5. **Services** (`#services`, cream) — six photo tiles (`.service-grid`): two columns on phones with the first and last tile full width, three columns from 768px. Under them, `.also` lists the extra services from the trailer as chips.
6. **Our work** (`#work`, striped lawn with grass blades along the top) — tilted polaroids (`.polaroids`) with handwritten captions. Swipe sideways on phones; a four-column grid from 768px.
7. **How it works** (`.steps`, cream) — three numbered steps.
8. **Quote form** (`#quote`, deep green with a slanted top) — name (required), phone (required), service dropdown, job description, yellow submit button.
9. **Footer** (`.site-footer`) — logo, tagline, Steve's name, phone, web address.
10. **Bottom bar** (`.cta-bar`, always on screen) — "Call Steve" and "Get a quote". A little mower (`.mow`) rides along the top of the bar and cuts the grass as the page scrolls.

### Behaviour (inline `<script>` at the bottom of `index.html`)
- **Scroll reveal:** headings, tiles, steps and the form slide in the first time they come on screen. The script adds `.rv`, then `.in`. Without JavaScript nothing is hidden.
- **Mower:** a scroll listener sets the CSS variable `--p` from 0 (top of page) to 1 (bottom).
- **Quote form:** sends to Formspree in the background (`fetch` with `Accept: application/json`), so the customer stays on the site.
  - While it sends, the button reads "Sending..." and is disabled.
  - On success, the form clears and "Thanks! Steve will be in touch soon." appears (`.form-status.success`).
  - On failure, a red message asks them to call, with a tap-to-call link (`.form-status.error`).
- **Reduced motion:** if the visitor's phone is set to reduce motion, every animation is switched off and the page is still.

---

## 3. Style guide (keep new work consistent with this)

### Colour palette
All colours, fonts and corner sizes are CSS variables in the `:root` block at the top of `style.css`. Use the variable (e.g. `var(--green)`), not a raw hex value.

The page alternates dark and light bands. Dark: header, hero, trust strip, footer, bottom bar. Light: services and how-it-works (cream). Feature bands: our work (lawn) and the quote form (deep green).

| Variable | Value | Where it's used |
|---|---|---|
| `--black` | `#0a0c0b` | Page background, header, footer, bottom bar |
| `--text` / `--text-muted` | `#f4f7f5` / `#a3ada8` | Text on dark areas |
| `--line` | `#262d29` | Dividers on dark areas |
| `--cream` | `#f4f1e6` | Services and how-it-works backgrounds |
| `--cream-text` / `--cream-muted` / `--cream-line` | `#10140f` / `#4c574f` / `#cfc9b6` | Text and borders on cream |
| `--paper` | `#fbfaf5` | Polaroid frames |
| **`--green`** | `#15834f` | Buttons, ticker, section labels. Dark enough for white text |
| `--green-bright` | `#34d38e` | Highlights on dark: "extra yard", dots, button glow |
| `--green-deep` | `#0b3a26` | Bottom of the quote section gradient |
| `--lawn` / `--lawn-stripe` | `#2da44e` / `#289a47` | Our-work stripes and grass blades |
| **`--sun`** | `#ffd23f` | Used sparingly: headline underline, form button, focus outline |
| `--plate`, `--plate-text`, `--logo-green`, `--logo-grey` | silver gradient etc. | The logo plate |
| Error (not a variable) | `#8a2a1c` text / `#fff4f2` background | Form error message only |

Three colours are written out inside SVGs because they can't read variables: the grass fill in `--grass` (same as `--lawn`), the underline stroke (same as `--sun`), and the seal's lime `#d7f25a`. Change them there if the palette changes.

### Typography (Google Fonts, loaded by the `<link>` in `index.html`)
- **Barlow** (`--font-body`, 400/600/700) — body text, buttons, tile titles.
- **Exo 2 italic** (`--font-display`, 700/800) — logo, section headings (uppercase), ticker, step numbers.
- **Kaushan Script** (`--font-script`) — the hero headline and polaroid captions only.
- Sizes in `rem`: hero headline `3.5` (→ `5` at 768px, `5.6` at 1000px), section heading `2` (→ `2.6`), tile title `1.05`, body `~1`, section label `0.78`.
- **Section label pattern:** every section starts with `<p class="eyebrow">` (a small slanted plate) followed by an `<h2>`.

### Shape and spacing
- Corners are `--radius` (8px) on buttons, tiles and inputs. Slanted edges (logo plate, section labels, ticker, top of the quote section) echo the trailer graphics.
- Side padding is `var(--gutter)`: 20px on phones, and it centres content at 1100px on wide screens. Use it instead of fixed side padding.
- Sections use `padding: 48px var(--gutter)`.
- `body` has `padding-bottom: 78px` so the bottom bar never covers the end of the page.

### Components to reuse
| Need | Use |
|---|---|
| Main action | `.btn` (green). Add `.call` for the phone button effects |
| Second action on a dark background | `.btn.ghost` (outline) |
| Stand-out action on green | `.btn.sun` (yellow) |
| New section | `<section class="…" id="…">` + `<p class="eyebrow">` + `<h2>` |
| New service | Copy an `<li>` in `.service-grid`: `img` + `div` (`h3` + one-sentence `p`). Add it to the form dropdown too |
| New job photo | Copy a `<figure>` in `.polaroids`: `img` + `figcaption` (two or three words) |
| Phone icon | `<svg><use href="#phone"/></svg>` |

### Motion
- Entrance effects run once; loops (slideshow, ticker, seal, ivy, button pulse) are slow and gentle.
- Anything new that moves must be covered by the `prefers-reduced-motion` block at the end of the MOTION section (it already switches off every animation and transition).

### Imagery
- Use real photos of Steve's work. Tiles and polaroids crop with `object-fit: cover`.
- **Files must really be JPEG/PNG**, about 1400px on the long side and under 300 KB. Don't just rename a file to `.jpg`.
- Photos of old prints need the paper border cropped off and the faded colour corrected before they go on the site.
- Filenames are lowercase and hyphenated, in `imgs/`.
- Every image has `alt` text describing what's in the photo.
- Don't use photos showing other businesses' signs or phone numbers.

### Voice and copy
- Friendly, plain and practical. Short sentences, no jargon, Australian spelling.
- Each service gets **one sentence**.
- Refer to Steve by name ("What Steve can help with", "Call Steve").
- The call to action is always worded "Get a quote".
- Phone number format: `0421 848 046` for display and `tel:0421848046` for links.

### Code conventions
- Two-space indentation.
- CSS is grouped under `/* UPPERCASE SECTION */` comments in page order. Add new rules under the matching comment.
- Class names are lowercase and hyphenated. States are extra classes (`.in`, `.success`, `.error`).
- Write mobile styles first. Overrides go in the `@media (min-width: 768px)` and `@media (min-width: 1000px)` blocks at the bottom.
- JavaScript stays inline at the end of `index.html`, inside the `DOMContentLoaded` handler, in plain vanilla JS.
- `html` has `overflow-x: hidden` and `body` has `overflow-x: clip`. Keep both: the ivy and ticker poke past the screen edge, and without these phones widen the page and push the bottom bar off screen.

---

## 4. Remaining needs

### Needs Steve's OK
- **New wording:** "Local and reliable", "Deal direct with Steve", the three how-it-works steps (especially "You know the price before work starts") and the polaroid captions were written for the redesign.
- **Service area and ABN** are not on the site because they weren't known. Add them to the footer; suburbs matter for "gardener near me" searches.

### Worth doing
- Favicon and Open Graph tags (the picture and title shown when the link is shared).
- Testimonials or Google reviews.
- Before/after pairs for pressure washing and solar cleaning.

### Unused files in `imgs/`
`pressure-washing.jpg` (stock photo) and `pressurewash2.jpg` are no longer shown on the page.

---

## 5. Checklist for any new change

- [ ] Colours and fonts come from the `:root` variables
- [ ] Side padding uses `var(--gutter)`
- [ ] Built mobile-first; wider-screen tweaks go in the `768px` / `1000px` media queries
- [ ] Checked at phone width (~375px), tablet (~768px) and desktop (~1440px)
- [ ] No sideways scrolling on a phone, and the bottom bar's two buttons both fit
- [ ] New images are real JPEG/PNG files, under 300 KB, in `imgs/`, with hyphenated names and their own `alt` text
- [ ] **New or removed services are updated in both the tiles and the form dropdown, with identical names**
- [ ] The quote form was **not** submitted as a test
- [ ] Every changed file is uploaded to GitHub, including new images
