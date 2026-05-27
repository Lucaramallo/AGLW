# Vortex-UI Round 1: Independent Expert Analysis

## Critical Design Constraints & Accessibility Imperatives

**1. Semantic HTML Foundation (Non-Negotiable)**
The search input must be a native `<input type="search">` with associated `<label>` (not placeholder-dependent), wrapped in a `<form>` to enable keyboard submission without JavaScript fallback. Current weather card needs `<article>` wrapper with ARIA landmarks; forecast grid requires `<ul>` or semantic `<section>` structure—not divs—because screen reader users need list semantics when parsing 5 forecast items. This prevents cognitive overload for assistive tech users.

**2. Responsive Grid vs. API Latency Mismatch**
Open-Meteo's geocoding endpoint (`https://geocoding-api.open-meteo.com/v1/search`) returns **multiple candidate cities** for ambiguous inputs (e.g., "Springfield"). UI must surface a **combobox pattern** (ARIA `role="combobox"` with keyboard navigation) not a dropdown—this is WCAG 2.1 AA compliant and survives touch devices. Forecast grid uses CSS Grid (`grid-template-columns: repeat(auto-fit, minmax(140px, 1fr))`) for mobile-first responsiveness, but **we must throttle API calls** to respect rate limits (default ~10 req/sec); debounce search input by 300ms minimum.

**3. Engineering Reality Check**
Open-Meteo returns ISO 8601 timestamps and WMO weather codes (not human text). Decoding WMO 80→"light rain" requires **hardcoded lookup table** embedded in JS (~100 lines). Timezone handling is **critical**: API returns UTC times; we cannot rely on `new Date()` parsing without explicit offset calculation. I recommend storing raw API response timestamps and rendering with explicit UTC awareness to prevent midnight-crossing bugs in forecasts.

**4. Visual Hierarchy Without Dependencies**
CSS Grid + Flexbox is sufficient; no Tailwind/Bootstrap needed. Key accessibility wins: (1) **color contrast minimum 4.5:1** for temp values and labels—test against WCAG AA, (2) **explicit focus states** with 3px outline (not browser default), (3) **relative font sizing** (rem-based) to respect user zoom preferences. Current weather card should use `<dl>` (definition list) for temperature/humidity pairs—semantic and screen-reader-friendly.

## My Position

**I'm proposing a monolithic single-file HTML solution** (with separate `<style>` and `<script>` blocks) that ships today, zero setup:
- HTML: semantic structure with ARIA labels
- CSS: Grid/Flexbox, 2 breakpoints (mobile < 640px, desktop ≥ 640px)
- JS: fetch wrapper with 300ms search debounce, WMO code lookup table, UTC-aware timestamp rendering

**Blocking risk**: Open-Meteo geocoding ambiguity requires **combobox dropdown**—Nexus-7 must validate if API returns `population` field for tie-breaking (it does; cities >100k population prioritized). Without this, search UX fails on common city names.

**Success metric**: Dashboard loads in <2s, search returns results in <400ms, forecast renders without layout shift (Cumulative Layout Shift < 0.1).