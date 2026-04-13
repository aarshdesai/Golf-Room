# The Golf Room — Dev Handoff

HTML/CSS development handoff package for **The Golf Room**, a premium golf simulation room booking platform.

## Quick Start

Open `index.html` in a browser — no build step, no dependencies, no server required.

```
open index.html
```

---

## What's in this repo

| File / Folder | Description |
|---------------|-------------|
| `index.html` | Hub page — links to design system and all page wireframes |
| `design-system.html` | Living style guide — all tokens and components with live demos |
| `globals.css` | **Source of truth** — all CSS custom properties (colors, spacing, typography, shadows, radii, layout) |
| `components.css` | All component classes (`.btn`, `.badge`, `.card`, `.input`, `.nav`, `.bay-card`, `.calendar`, `.modal`, etc.) |
| `pages/` | 8 HTML page wireframes — fully styled, standalone references |
| `assets/logos/` | 6 brand logo SVGs (light/dark variants for wordmark, monogram, and badge) |

---

## Pages

| File | Route | Description |
|------|-------|-------------|
| `pages/landing.html` | `/` | Marketing homepage — hero, features, membership, testimonials, auth modal |
| `pages/bays.html` | `/bays` | Bay browser — filter bar, 2-col grid, detail sidebar |
| `pages/booking.html` | `/book` | Full 3-step booking wizard (calendar → bay → review) |
| `pages/booking-datetime.html` | `/book` | Step 1 detail — date/time picker with sidebar |
| `pages/booking-confirm.html` | `/book/confirm` | Confirm & pay — session details + payment form |
| `pages/dashboard.html` | `/dashboard` | Authenticated hub — stats, next session, history |
| `pages/confirmation.html` | `/confirmation/:id` | Post-booking — confirmation + QR code |
| `pages/profile.html` | `/profile` | Edit profile form |

---

## Design System

### Stack
- Plain HTML5 + CSS Custom Properties
- No framework, no preprocessor, no build step
- Google Fonts: Cormorant Garamond, DM Sans, DM Mono
- Responsive at `640px` (mobile) and `768px` (tablet)

### Theming
- **Light mode** is default (`:root`)
- **Dark mode** via `[data-theme="dark"]` on `<html>`
- Theme persisted in `localStorage` under key `golf-room-theme`

```js
// Init theme before paint (place in <head>)
const stored = localStorage.getItem('golf-room-theme');
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
if (stored === 'dark' || (!stored && prefersDark)) {
  document.documentElement.setAttribute('data-theme', 'dark');
}
```

### Logo switching pattern
```html
<!-- Favicon -->
<link rel="icon" href="assets/logos/mogogram-logo-dark.svg"  type="image/svg+xml" media="(prefers-color-scheme: light)">
<link rel="icon" href="assets/logos/mogogram-logo-light.svg" type="image/svg+xml" media="(prefers-color-scheme: dark)">

<!-- Nav wordmark (swap on theme toggle) -->
<img src="assets/logos/the-golf-room-logo-dark.svg"  id="logo-light" alt="The Golf Room">
<img src="assets/logos/the-golf-room-logo-light.svg" id="logo-dark"  alt="The Golf Room" style="display:none">
```

### Using the stylesheets
```html
<link rel="stylesheet" href="../globals.css">     <!-- tokens first -->
<link rel="stylesheet" href="../components.css">  <!-- then components -->
```

All component classes consume CSS variables from `globals.css`. Never hardcode hex values, spacing, or font families.

### Key tokens
```css
/* Backgrounds */
var(--color-bg-base)         /* page background */
var(--color-bg-surface)      /* cards */
var(--color-bg-elevated)     /* hover states, raised elements */
var(--color-bg-overlay)      /* nav, modals — semi-transparent glass */

/* Brand */
var(--color-green-primary)   /* #1B6844 — buttons, borders, icons */
var(--color-gold)            /* primary CTA color */

/* Text */
var(--color-text-primary)
var(--color-text-secondary)

/* Layout */
var(--nav-height)            /* 72px — always pad page content by this */
var(--slot-h)                /* 64px — calendar hour-block height */
```

---

## Component Classes

Full demos at `design-system.html`. Quick reference:

```html
<!-- Buttons -->
<button class="btn btn--primary">Book Now</button>
<button class="btn btn--secondary">Learn More</button>
<button class="btn btn--ghost">Cancel</button>
<button class="btn btn--primary btn--sm">Small</button>
<button class="btn btn--primary btn--lg">Large</button>

<!-- Badges -->
<span class="badge badge--green">Available</span>
<span class="badge badge--gold badge--dot">Premium</span>
<span class="badge badge--error">Booked</span>

<!-- Card -->
<div class="card">...</div>

<!-- Input -->
<input class="input" type="text" placeholder="...">
<input class="input input--error" type="email">
<select class="select">...</select>

<!-- Bay card -->
<div class="bay-card bay-card--highlands">...</div>
<div class="bay-card bay-card--masters bay-card--selected">...</div>
<div class="bay-card bay-card--links bay-card--unavailable">...</div>

<!-- Time slot -->
<div class="time-slot">10:00</div>
<div class="time-slot time-slot--selected">11:00</div>
<div class="time-slot time-slot--unavailable">13:00</div>

<!-- Filter chip -->
<div class="filter-chip filter-chip--active">Highlands</div>

<!-- Booking step indicator -->
<div class="booking-steps">
  <div class="step step--complete"><div class="step__circle">✓</div><span class="step__label">Date</span></div>
  <div class="step-connector step-connector--complete"></div>
  <div class="step step--active"><div class="step__circle">2</div><span class="step__label">Bay</span></div>
  <div class="step-connector"></div>
  <div class="step"><div class="step__circle">3</div><span class="step__label">Confirm</span></div>
</div>
```

---

## Figma

[The Golf Room — Figma File](https://www.figma.com/design/PaHzEIE6iPPmU2mF38aelc/Golf-Room)

---

## Rules

- **Never hardcode** hex values, spacing, or font families — always use CSS variables
- **No Tailwind, SCSS, or inline styles** (except for runtime-dynamic values)
- **All clickable elements** must have `cursor: pointer`
- **Hover states** must have 150–300ms `ease` transitions
- **Page content** must use `padding-top: var(--nav-height)` to clear the fixed nav
- **Responsive** at 640px (mobile) and 768px (tablet)
- **Icons** — inline SVG only, no icon libraries
