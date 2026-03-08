# LifeTracker

A personal life-tracking web app that runs entirely as a static site on GitHub Pages. No backend, no build step, no external APIs. All data lives in `localStorage`. The app is a collection of standalone HTML files — a home dashboard linking out to dedicated trackers for different areas of life.

---

## File Structure

```
index.html          Home dashboard
fitness.html        PPL workout tracker (Push / Pull / Legs)
hunter-stats.html   Gamified fitness stats companion (Solo Leveling theme)
habits.html         Daily habit tracker with streaks
media.html          Movies, TV shows, and books tracker
restaurants.html    Restaurant tracker (Beli-style)
travel.html         Places visited and want-to-visit tracker
README.md           This file
```

---

## Pages

### Home Dashboard (`index.html`)
- Time-aware greeting ("Good morning", "Good afternoon", "Good evening")
- Pulls live stats from each tracker's localStorage and displays them as summary cards
- Each card is a tappable link to its tracker page
- Today strip showing the current date, a habit streak badge (if active), and a nudge if habits are incomplete
- Settings panel (top-right gear icon) to set your name, stored in localStorage

### Fitness — PPL Tracker (`fitness.html`)
A full Push / Pull / Legs workout tracker with a dark, high-contrast aesthetic (Barlow Condensed / Barlow fonts, dark surface palette).

- **Push, Pull, Legs, Abs panels** — each lists exercises with sets/reps, rest times, target muscles, and expandable technique notes
- **Log tab** — streak cards (current streak, total sessions, this week), a backlog entry form to log any past date, and a navigable monthly calendar with color-coded workout dots
- **Muscle summary** — bar chart of muscle groups trained in the last 7 days
- **Session history** — last 60 sessions with delete support
- Links to the Hunter Stats companion page
- Back button (← Home) returns to `index.html`

### Hunter Stats (`hunter-stats.html`)
A gamified stats page companion to the PPL tracker, themed after Solo Leveling.

- **Stats tab** — animated hunter profile card with rank badge (E through S), total XP, overall rank progress bar, and per-muscle attribute levels visualized on an anime-style SVG body diagram with glowing connectors
- **Quests tab** — daily, weekly, and milestone quests with progress bars and XP rewards
- **Feats tab** — achievement grid that unlocks as milestones are hit, with toast-style unlock notifications
- Reads from the same `ppl_v3` localStorage key as `fitness.html`
- Back button (← PPL) returns to `fitness.html`

### Habits (`habits.html`)
- Define up to 10 habits with a name, emoji, and frequency (daily or specific days of the week)
- **Today tab** — large tappable habit rows with live streak counts; tap to mark complete, tap again to unmark
- **This week tab** — 7-day completion grid (rows = habits, columns = days), today highlighted
- **Manage tab** — add, edit, and delete habits
- Stats strip: best streak, total completions this month, week completion rate

### Media (`media.html`)
- Three tabs: Movies, TV, Books
- Each entry: title, creator (director/author), genre, rating (1–10), status, date added, review/notes
- TV shows track seasons watched as a separate field
- Filter by status (Watched, Want to Watch, DNF, etc.)
- Stats strip: total, completed count, added this year, average rating
- Floating + button quick-add, tap any card to edit, delete from edit modal

### Restaurants (`restaurants.html`)
- Restaurant cards: name, cuisine, neighborhood, rating (1–10), status (Visited / Want to Go / Favorite), date added, notes
- Named lists: Visited, Want to Go, Favorites (defaults), plus unlimited custom lists
- Search bar and filter panel (cuisine, city, min rating, sort order)
- Stats view: total tracked, average rating, top cuisines, cities breakdown
- **Import:** Google Takeout JSON, generic JSON arrays, or CSV — with preview before confirming
- **Export:** JSON backup download
- Tap any card to edit, delete from edit modal

### Travel (`travel.html`)
- Each entry: city, country, country code (for flag emoji), date visited (month/year), trip type (solo, partner, family, friends, work), rating, notes, status (Visited / Want to Go)
- Grid card layout with auto-generated flag emojis from country code
- World regions breakdown: auto-calculates continent from country name
- Stats strip: cities visited, countries, continents, most recent trip
- Filter by status, trip type; search by city or country
- Export and import JSON backup

---

## Design System

| Token | Value |
|---|---|
| Background | `#F9F7F4` (warm off-white) — light pages |
| Card | `#FFFFFF` |
| Text | `#1A1A1A` |
| Muted text | `#888880` |
| Accent | `#C4714A` (dusty terracotta) |
| Accent light | `#F5E8E0` |
| Border | `#EDEBE7` |
| Heading font | DM Serif Display |
| Body font | DM Sans |
| Border radius | 16px (cards), 12px (inputs/buttons) |

The fitness pages (`fitness.html`, `hunter-stats.html`) use their own independent dark theme inherited from the original PPL tracker (Barlow Condensed / Barlow, `#0d0d0d` background).

---

## localStorage Keys

| Key | Used by |
|---|---|
| `ppl_v3` | `fitness.html`, `hunter-stats.html` |
| `hunter_ach` | `hunter-stats.html` (achievement unlock state) |
| `lifetracker_v1_habits` | `habits.html`, `index.html` |
| `lifetracker_v1_media` | `media.html`, `index.html` |
| `lifetracker_v1_restaurants` | `restaurants.html`, `index.html` |
| `lifetracker_v1_travel` | `travel.html`, `index.html` |
| `lifetracker_v1_settings` | `index.html` (user name) |

---

## Technical Notes

- Every `.html` file is fully self-contained with inline CSS and JS — no external dependencies except Google Fonts and Lucide icons (both loaded via CDN, pinned versions)
- Lucide icons: `cdn.jsdelivr.net/npm/lucide@0.383.0/dist/umd/lucide.min.js`
- Google Fonts: DM Serif Display + DM Sans (light pages), Barlow Condensed + Barlow (fitness pages), Rajdhani + Orbitron (hunter stats page)
- No frameworks, no npm, no build step — open any file directly in a browser or serve from GitHub Pages
- Works offline after first load (CDN resources cached by browser)
- localStorage quota errors are caught and surface a user-facing message

---

## Deployment

1. Put all files in the root of a GitHub repository
2. Go to **Settings → Pages**, set source to the `main` branch, root folder
3. GitHub Pages will serve the site at `https://yourusername.github.io/your-repo-name/`

No configuration needed.

---

## Changelog

### v1.0.0 — Initial build
- Created `index.html` home dashboard with time-aware greeting, per-tracker live stat cards, today strip with habit nudge and streak badge, and settings panel for user name
- Created `habits.html` with Today / This Week / Manage tabs, streak tracking, 7-day grid, and stats strip
- Created `fitness.html` generic workout logger (later replaced — see v1.1.0)
- Created `restaurants.html` with Beli-style cards, custom lists, Google Takeout / CSV import, stats view, and JSON export
- Created `media.html` with Movies / TV / Books tabs, status filters, and stats strip
- Created `travel.html` with grid card layout, flag emoji, world regions breakdown, and JSON backup
- Shared design system: DM Serif Display + DM Sans, dusty terracotta accent (`#C4714A`), warm off-white background

### v1.0.1 — Lucide icon CDN fix
- Replaced `unpkg.com/lucide@latest` (unreliable `@latest` resolution) with pinned version `cdn.jsdelivr.net/npm/lucide@0.383.0/dist/umd/lucide.min.js` across all six pages
- Added `if(window.lucide)` guard before every `lucide.createIcons()` call so a CDN failure degrades gracefully instead of throwing and crashing the page

### v1.1.0 — Fitness tracker replaced with PPL workout app
- Replaced generic `fitness.html` with a full Push / Pull / Legs workout tracker (original dark-theme app)
- Added `hunter-stats.html` as a gamified companion page (Solo Leveling theme) with muscle attribute SVG diagram, rank system (E→S), quests, and achievements
- Both pages use `ppl_v3` as their shared localStorage key
- Added `← Home` back button to `fitness.html` top bar linking to `index.html`
- Updated `← PPL` back button in `hunter-stats.html` to correctly point to `fitness.html`
- Updated home dashboard (`index.html`) fitness stat card to read from `ppl_v3` instead of `lifetracker_v1_fitness`, now displays weekly sessions and current streak
