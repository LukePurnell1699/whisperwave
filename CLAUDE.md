# WhisperWave — ASMR Live Platform

## Project Overview

WhisperWave is a multi-page front-end website for a live ASMR streaming platform. It is built entirely in vanilla HTML, CSS, and JavaScript — no frameworks, no build tools, no dependencies beyond Google Fonts. All pages are self-contained `.html` files designed to be served statically (e.g. GitHub Pages).

## File Structure

```
Downloads/
├── index.html           # Homepage / landing page
├── browse.html          # Creator discovery & filtering
├── room.html            # Live stream room (chat, gifts, interactions)
├── tokens.html          # Token store & checkout
├── become-creator.html  # Creator onboarding & application form
└── CLAUDE.md            # This file
```

## Design System

### Colour Palette
All colours are defined as CSS custom properties in `:root` on every page:

| Variable    | Value                     | Usage                          |
|-------------|---------------------------|--------------------------------|
| `--cream`   | `#f5efe6`                 | Primary text, backgrounds      |
| `--blush`   | `#e8c9b0`                 | Italic accents, hover states   |
| `--amber`   | `#c8894a`                 | Brand colour, CTAs, tokens     |
| `--ember`   | `#9b5a2a`                 | Gradient pairs with amber      |
| `--velvet`  | `#1a0f0a`                 | Page background (near-black)   |
| `--smoke`   | `#2e1c14`                 | Panel backgrounds, cards       |
| `--mist`    | `rgba(245,239,230,0.06)`  | Subtle borders, overlays       |
| `--glow`    | `rgba(200,137,74,0.18)`   | Ambient glow effects           |

### Typography
- **Headings:** `Cormorant Garamond` — serif, weight 300/400, often italic for emphasis
- **Body / UI:** `Jost` — sans-serif, weight 200/300/400
- Both loaded via Google Fonts

### Spacing & Layout
- Max content width: `1100px` (browse/featured grids), `900px` (forms/tokens), `680px` (single-column text)
- Section padding: `6–8rem` vertical, `2–3rem` horizontal
- Border radius: `3–6px` for cards/panels, `3rem` for pill buttons
- Grid gaps: `1–2rem` for card grids, `5rem` for two-column feature layouts

### Buttons
```css
.btn-primary        /* amber fill, dark text, pill shape */
.btn-ghost          /* transparent, muted text, arrow icon */
.btn-nav-primary    /* nav-sized amber pill */
.btn-nav-ghost      /* nav-sized transparent text link */
```

### Animations
- `fadeUp` — entrance animation for hero elements (opacity + translateY)
- `breathe` — slow scale pulse on ambient glows
- `pulse` — live indicator red dot
- `wave` / `vizWave` — waveform bars
- `msgIn` — chat message entrance
- `giftIn` / `popIn` — gift send feedback
- `barGrow` — earnings bar fill animation

## Page-by-Page Notes

### index.html
- Full-height hero with animated waveform and live badge
- Featured creator section (two-column: visual + content)
- 8-card streamer grid with colour-coded gradient backgrounds (`.c1`–`.c8`)
- Atmosphere quote section with three pillars
- 6-category grid (Whisper, Touch, Breath, Rain, Crinkle, Roleplay)
- Three testimonial cards
- CTA section + footer

### browse.html
- Nav includes token balance pill linking to `tokens.html`
- Filter buttons (JS toggles `.active` class) and sort `<select>`
- Two grid rows: "Featured Live" (10 cards) and "Rising Creators" (5 cards)
- Creator cards use `.c1`–`.c12` gradient classes
- Badge system: `.badge-top`, `.badge-new`, `.badge-priv`
- "Become a Creator" strip at the bottom

### room.html
- **Layout:** Fixed `topbar` + `room-layout` grid (stream area | 340px sidebar)
- **Stream area:** Animated ambient particles (JS-generated), SVG silhouette, creator glow, waveform visualizer (60 JS-generated bars)
- **Interaction bar:** 6 interaction buttons with token costs (Wave = free, Whisper = 10◈, Name = 25◈, Attention = 50◈, Ear-to-Ear = 100◈, Private = 500◈)
- **Sidebar tabs:** Chat / Gifts / About
  - Chat: scrolling messages, simulated live chat (setInterval), send on Enter or button
  - Gifts: 10 gifts from 30◈ (Warm Breath) to 5,000◈ (Eternal Ember); sending triggers flash overlay + toast + chat message
  - About: creator bio, tags, stats, private session button
- **Token system:** `tokens` variable in JS, `updateTokenDisplay()` syncs all displays, `spendTokens(amount, action)` deducts and validates
- **Toast notification:** `.toast` element, `.show` class added/removed for 3s
- **Gift flash:** Full-screen overlay emoji that pops in and fades

### tokens.html
- Current balance banner
- 4 token packages in a grid (100◈/$1.99 → 6,500◈/$39.99)
- Best value badge on the 1,250◈ package
- Checkout modal triggered by `openCheckout()` — simulated card form
- Purchase success fullscreen overlay triggered by `completePurchase()`
- "What tokens unlock" grid (6 use cases with costs)
- "How it works" 3-step section
- Trust bar (secure, instant, no-expiry, refund)

### become-creator.html
- Hero with 3 live stats (avg earnings, revenue share %, listener count)
- Earnings section: left = breakdown table (70% on all streams), right = animated payout example visual
- 6 creator perks grid
- 4-step getting-started list
- Creator testimonials with earnings figures
- Application form:
  - Name, email, creator name, primary style `<select>`
  - Tag multi-select (`.tag-opt` toggled via JS)
  - Bio textarea
  - Referral source `<select>`
  - Submit triggers success overlay

## Token Economy

| Action                  | Cost       |
|-------------------------|------------|
| Wave (free)             | 0◈         |
| Whisper Request         | 10◈        |
| Say My Name             | 25◈        |
| Personal Attention      | 50◈        |
| Ear-to-Ear              | 100◈       |
| Private Session         | 500◈       |
| Gift — Warm Breath      | 30◈        |
| Gift — Soft Petal       | 50◈        |
| Gift — Moon Drop        | 75◈        |
| Gift — Candlelight      | 100◈       |
| Gift — Crystal Rain     | 250◈       |
| Gift — Velvet Rose      | 300◈       |
| Gift — Night Bloom      | 400◈       |
| Gift — Golden Whisper   | 500◈       |
| Gift — Throne of Sound  | 2,000◈     |
| Gift — Eternal Ember    | 5,000◈     |

Token packages: 100◈ ($1.99) · 550◈ ($4.99) · 1,250◈ ($9.99) · 6,500◈ ($39.99)

Creator revenue share: **70%** of all token spend in their room.

## Navigation & Linking

All internal links are relative `.html` file references:

```
index.html      → browse.html, room.html
browse.html     → room.html, become-creator.html, tokens.html, index.html
room.html       → browse.html, tokens.html, index.html
tokens.html     → browse.html, index.html
become-creator.html → index.html
```

## Conventions

- **No JavaScript frameworks** — all JS is vanilla, inline in `<script>` tags at the bottom of each file
- **No external CSS files** — all styles are in a `<style>` block in each page's `<head>`
- **Simulated interactivity** — no backend; purchases, gifts, and chat are all client-side simulations
- **Consistent noise texture** — every page uses the same inline SVG `feTurbulence` filter as `body::before`
- **Consistent ambient glow** — every page uses the same `body::after` radial gradient + `breathe` animation (except `room.html` which is full-height locked)
- **Card colour classes** `.c1`–`.c12` — radial gradient + linear-gradient backgrounds used on creator cards across browse and index pages. Add new ones by following the same pattern.
- **Section labels** — small uppercase amber text above every section heading, using `.section-label`

## Extending the Site

### Adding a new creator card
Copy any `.streamer-card` or `.creator-card` block and change the colour class (`.c1`–`.c12` or add a new one), name, genre, and viewer count.

### Adding a new gift
In `room.html` gifts panel, copy a `.gift-item` block and call `sendGift(this, 'Name', '🎁', tokenCost)` in the `onclick`. Add matching token cost to the table above.

### Adding a new page
1. Copy the `<nav>`, `<footer>`, and shared CSS variables/body styles from any existing page
2. Add a link in the nav of relevant pages
3. Follow the section structure: `.section-label` → `h2.section-title` → content

### Deploying
Static files — deploy anywhere that serves HTML:
- **GitHub Pages:** push to `main`, enable Pages in repo Settings → Pages
- **Netlify / Vercel:** drag and drop the folder or connect the repo
- **URL pattern (GitHub Pages):** `https://USERNAME.github.io/REPO_NAME/`

## Known Limitations (for future development)

- No real authentication — sign in / account buttons are non-functional placeholders
- No real audio/video — stream viewport is visual-only; would need WebRTC for live audio
- No real payments — Stripe checkout is a UI mock; needs Stripe.js integration
- No real-time chat — uses `setInterval` simulation; would need WebSockets or a service like Pusher
- Token balance resets on page refresh — would need `localStorage` or a backend session
- Creator dashboard page not yet built — application form submits but has no admin review UI
