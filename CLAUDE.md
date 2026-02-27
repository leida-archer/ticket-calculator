# Music Event Financial Planner — CLAUDE.md

## Project Overview
Single-file PWA for planning music event finances. Calculates ticket prices based on talent, production, venue, staffing, marketing costs, headcount, and profit margin.

## Stack
- Single `index.html` (HTML + CSS + JS, no build step)
- PWA: `manifest.json`, `sw.js` (network-first caching), icons (192/512)
- Dark theme (#111 background, #1a1a1a cards, #333 borders)
- Hosted on GitHub Pages (repo: wafflegeek/ticket-calculator)

## Architecture
- Two-column layout: left column (inputs), right column (sticky results)
- Left column has two `.card` panels: Event Details card + Costs card
- Stacks vertically on mobile (<640px)
- All state is in the DOM — no framework, no localStorage

## UI Patterns

### Show/Hide Dropdowns
Used for Lineup, Venue Staffing, Production Staffing:
1. Hidden `*ToggleWrap` div + visible `+ Add` button (initial state)
2. Clicking `+ Add` shows the wrap, hides the button, opens dropdown, adds first entry
3. Removing last entry auto-hides wrap and re-shows the `+ Add` button
4. Dropdown uses `.bar-fields` with CSS `max-height` transition + `.open` class

### Collapsible Sections
- `.bar-toggle` with `.arrow` span (▶) that rotates 90deg when `.open`
- Arrow MUST have `display: inline-block` for CSS transform to work
- Production Hardware Cost is always-visible (no show/hide pattern)

### Currency Inputs
Wrapped in `<span class="currency-wrap"><span class="dollar">$</span><input ...></span>`

### Section Order (Left Column)
1. **Event Details card**: Event Date, Start Time, End Time
2. **Costs card**:
   - Talent Costs (h2) → Lineup dropdown (show/hide)
   - Production Costs (h2) → Venue Fee, Bar Minimum, Venue Staffing (show/hide), Production Hardware Cost (always visible dropdown), Production Staffing Cost (show/hide)
   - Marketing Costs (h2) → Paid Ads/Week, Content Generation/Week
   - Attendance Goal (h2) → Expected Headcount, Profit Margin slider

### Results (Right Column)
- Cost breakdown lines, Total Costs, Target Revenue, Profit, Ticket Price (green)
- Conditional: Bar Minimum Breakeven (if barMin > 0)
- Conditional: Venue Staffing line (if staff added)

## Naming Conventions
- Title Case on all labels/headings EXCEPT `+ Add` buttons (lowercase after +)
- Section headers use `<h2>` with uppercase CSS transform

## Calculations
- Event duration handles overnight (e.g. 8pm–2am = 6 hours)
- Venue staffing = hourly rate × event hours × team size
- Marketing = weekly cost × weeks until event date
- Total = talent + travel + production + venue + staffing + ads + content
- Ticket price = total × (1 + margin%) / headcount

## Service Worker
- Cache name: `ticket-calc-v4` — bump version on deploy
- Network-first strategy with cache fallback

## Deployment
- `gh-pages` via GitHub Pages
- Bump `sw.js` CACHE version before pushing
