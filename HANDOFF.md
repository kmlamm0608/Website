# AAC Steezy — Designer Handoff

**Client:** Active Apparel Co. (AAC)
**Project:** Sale campaign landing page — "Midseason Sale, 40% off sitewide"
**Deliverable:** Static, single-file site. No framework, no build step, no npm. External deps: Google Fonts + Unsplash imagery only.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | Landing page — all HTML, CSS, and JS inline, commented by section |
| `404.html` | Custom 404 page, same design system |

Both run by double-clicking. Deploy by uploading the folder to any static host (Netlify Drop, GitHub Pages, Vercel).

---

## Design system

### Color tokens (defined in `:root`)

| Token | Value | Role |
|-------|-------|------|
| `--ink` | `#262626` | Deep charcoal — text, dark sections, buttons |
| `--ink-2` | `#333333` | Hover tier of ink |
| `--ink-soft` | `#575757` | Secondary text on light surfaces |
| `--pink` | `#f5a0bd` | **The signature.** Brand placard, CTA fill, accents |
| `--pink-deep` | `#e087ab` | Pink hover tier |
| `--rose-text` | `#a8446b` | Darkened pink — AA-compliant small text on light bg |
| `--blush` | `#fbe4ec` | Pink-tinted section fill |
| `--paper` | `#f4f2ee` | Warm page canvas |
| `--plaster` | `#eceae5` | Card surface tier (cards step, never float) |
| `--white` | `#ffffff` | Card surfaces on tinted sections |

Rules: 2 brand colors (ink + pink) plus neutrals. Pink is reserved for activation moments (CTAs, placard, accents) — roughly 3–5% of any screen. Everything else is achromatic discipline.

### Typography

| Family | Use |
|--------|-----|
| **Archivo** (variable width 62–125, weights 400–900) | Display. `font-stretch: 118–122%` + `font-weight: 900` + uppercase = the mega headline voice. Hero is lowercase with `-0.03em` tracking (editorial sculpted-mass look) |
| **Inter** (400–700) | All body copy, nav, buttons |
| **Space Mono** (400/700) | Technical metadata only: countdowns, dates, badges, error tags — the "spec sheet" voice |

### Type scale (fluid)

- Hero title: `clamp(2.7rem, 8vw, 6.9rem)`
- Section titles: `clamp(2rem, 4.6vw, 3.6rem)`
- Card titles: `1.1rem` uppercase Archivo 900
- Body: `16px` / line-height `1.6`
- Micro labels: `0.72rem`, letter-spacing `0.24em`, uppercase

### Shape & elevation

- Cards/sections: `10px` radius (`--r`)
- Pills/buttons/badges: `999px`
- **No drop shadows on cards.** Separation comes from surface color stepping (paper → plaster → white), per the Zellerfeld-inspired gallery discipline. Shadows only on hover-lift for buttons.

### Motion

- One IntersectionObserver pass: `.reveal` elements fade/rise once (`--i` custom property staggers them)
- Hero headline: masked line-rise on load (`.line > span`)
- Manifesto: scroll-fill stroke text (`.fill-line` gains `.lit`)
- Marquee: single pink band, CSS `translateX` loop, pauses on hover
- Dock: sticky sale pill, appears after hero CTA exits viewport, steps aside near the final CTA
- All motion gated by `prefers-reduced-motion` (reveals render instantly, marquee stops, dock hides)

---

## Layout structure (index.html, in order)

1. Skip link (a11y)
2. Announcement bar (ink, pink highlights, mono date)
3. Sticky header — brand placard, 3 nav links, pink pill CTA → `https://gcu.edu`
4. Mobile overlay menu (full-screen ink, numbered mega links, Escape/backdrop close, focus moves to Close)
5. Hero — full-bleed court photo (Unsplash `photo-1519766304817`), pink kicker pill, 2-line kinetic H1, supporting sub, two CTAs
6. Pink marquee band (aria-hidden, duplicated spans for seamless loop)
7. Benefits — 4 plaster cards with watermark numerals (01–04), hover fills white
8. Manifesto — ink section, 3-line scroll-fill headline + mono footline
9. Single review card ("Receipts, not hype.") — deliberately one strong verification
10. Final CTA — ink field, pink radial "sun", date pill, gcu.edu button
11. Footer — giant wordmark, section links, dynamic `#copy-year`
12. Dock — fixed sale pill with live countdown, `https://gcu.edu` button

**CTAs point to `https://gcu.edu`** (assignment requirement — the client's designated destination). All five: header, hero, mobile menu, final, dock.

---

## Assignment constraints (do not break)

1. Sale headline with urgency (hero H1)
2. Supporting headline (hero sub)
3. Strong visual focal point (hero photo)
4. Core benefits (4-card band)
5. Prominent CTA → `https://gcu.edu`
6. Stated sale end date ("Sunday, October 4 · 11:59 PM" — computed live to next Sunday)
7. Clean legible typography
8. Accessible contrast (ink on paper ≈ 13.9:1; `--rose-text` is the AA-safe pink for small text)
9. Persona-appropriate palette
10. **150–200 words of promotional copy — currently 198.** Any copy edit must be re-counted (count excludes nav, buttons, footer legal, announcement bar)
11. Clear intuitive UX

## JS behavior (vanilla, ~120 lines)

- Countdown: `nextSaleEnd()` rolls to upcoming Sunday 23:59:59 local; renders `4D 10:05:37` format into the dock
- Copy-year stamp
- Hero kinetic trigger, reveal observer, manifesto filler observer, dock show/hide (IntersectionObserver on hero actions + final actions)
- Mobile menu open/close with `aria-expanded`, body scroll lock, Escape key

## Editing notes

- All styles live in one `<style>` block, organized in numbered commented sections — search by section number
- Images: Unsplash URLs with `w=`/`q=` params — swap by replacing the photo ID, keep params
- The marquee is `aria-hidden` and duplicated — edit both spans (word-count counter excludes repeats past span 4)
- Favicon is an inline SVG data URI (pink placard + AAC) — mirrors the header brand mark
