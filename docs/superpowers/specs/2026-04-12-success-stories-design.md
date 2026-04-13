# Success Stories Section — Design Spec

**Date:** 2026-04-12
**Status:** Approved
**Scope:** Add a "Real Results" success stories section to CivicConnect

---

## Overview

A curated success stories section where real users share how CivicEngage helped them get results from their elected officials. Stories are submitted via Google Form, manually reviewed, and committed as JSON + images to the repo. Displayed as cards in a standalone section above the footer, visible to all visitors without requiring an address search.

**Purpose:** Social proof that civic engagement works. Visitors see real outcomes before or after using the tool — builds trust and motivation to take action.

---

## Decisions Made

| Decision | Choice | Reasoning |
|----------|--------|-----------|
| Content source | Curated by maintainer via Google Form submissions | No backend needed; maintainer controls quality |
| Required fields | Title + text only | Low friction to submit; optional fields add richness over time |
| Text limit | 500 characters | 3-4 sentences — enough to tell a story, short enough to scan |
| Image storage | In-repo at `data/images/stories/` | Simple; compress before commit; migrate later if needed |
| Placement | Standalone section above footer | Social proof visible without address search |
| Card interaction | Click opens modal | Reuses existing modal pattern (FAQ, About, Support, Rep Profile) |
| Layout | 2-column asymmetric masonry (desktop), single column (mobile) | Avoids generic 3-col grid; feels editorial |

---

## Data Structure

### `data/stories.json`

Array of story objects, fetched at runtime (same pattern as `data/leaders.json`):

```json
[
  {
    "id": "pothole-burlington-2026",
    "title": "Pothole Fixed in 48 Hours",
    "text": "After using CivicEngage to find my councillor, I sent one email describing the pothole on Brant Street. Got a call back the next day, crew was out within 48 hours.",
    "category": "Infrastructure",
    "city": "Burlington, ON",
    "rep_contacted": "Lisa Kearns",
    "days_to_resolve": 2,
    "satisfaction": "great",
    "screenshot": "images/stories/pothole-burlington.jpg",
    "date": "2026-03-15",
    "author": "Anonymous"
  }
]
```

**Field requirements:**

| Field | Required | Type | Notes |
|-------|----------|------|-------|
| `id` | Yes | string | URL-safe slug, unique |
| `title` | Yes | string | Short headline |
| `text` | Yes | string | Max 500 characters |
| `category` | No | string | e.g. "Infrastructure", "Policy", "Response", "Services" |
| `city` | No | string | City + province, e.g. "Burlington, ON" |
| `rep_contacted` | No | string | Name of the rep they reached out to |
| `days_to_resolve` | No | number | Days from contact to resolution |
| `satisfaction` | No | string | One of: `"great"`, `"good"`, `"mixed"` |
| `screenshot` | No | string | Path relative to `data/`, e.g. `images/stories/filename.jpg` |
| `date` | No | string | ISO date (YYYY-MM-DD) |
| `author` | No | string | Display name or "Anonymous" |

### Image Storage

- Location: `data/images/stories/`
- Compress/resize before committing (target: <200KB per image)
- Supported formats: JPG, PNG, WebP
- Referenced in JSON as relative path from `data/`

---

## Submission Pipeline

1. Google Form collects: title, story text, category (dropdown), city, rep name, resolution time, satisfaction (dropdown), screenshot upload, author name
2. Maintainer receives email notification on submission
3. Maintainer reviews, compresses screenshot if needed, adds entry to `data/stories.json`, commits image to `data/images/stories/`
4. Push to main deploys via GitHub Pages

---

## UI Design

### Section Layout

Standalone section between the last content area and the footer. Always visible.

```
Section heading: "Real Results" (DM Serif Display)
Subtitle: "People who took action — and got heard." (DM Mono, var(--text-muted))

[2-column asymmetric masonry grid of story cards]

[Share Your Story ->] button (accent, links to Google Form)
"See all N stories ->" link (if more than 5 stories exist)
```

**Desktop (>480px):** 2-column grid with `gap: 1rem`. Cards have natural height based on content, creating a masonry-like stagger.

**Mobile (<=480px):** Single column, full-width cards.

**Max visible cards:** 5 initially. If more exist, remaining cards are hidden with CSS (`display: none`). A "See all N stories" text button toggles them visible via JS (adds/removes a `.stories-expanded` class on the container). No separate page needed yet.

### Card Design

```css
.story-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-left: 4px solid var(--accent);
  border-radius: 20px;
  padding: 1.5rem 1.8rem;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.10);
  cursor: pointer;
  transition: border-color 0.25s cubic-bezier(0.16, 1, 0.3, 1),
              box-shadow 0.25s cubic-bezier(0.16, 1, 0.3, 1),
              transform 0.25s cubic-bezier(0.16, 1, 0.3, 1);
  animation: fadeUp 0.4s ease both;
}

.story-card:hover {
  border-color: var(--accent);
  box-shadow: 0 8px 28px rgba(0, 0, 0, 0.16);
  transform: translateY(-3px);
}
```

**Card content (top to bottom):**

1. **Meta line:** Category pill + city (DM Mono, 0.65rem, uppercase, `var(--text-muted)`)
   - Category rendered as subtle pill: `var(--border)` background, `border-radius: 12px`, `padding: 0.15rem 0.6rem`
2. **Title:** DM Serif Display, 1rem, `var(--text)`
3. **Excerpt:** DM Mono, 0.75rem, `var(--text-muted)`, truncated to ~120 characters with ellipsis
4. **Footer line:** Satisfaction dot + resolution time + date + camera SVG icon (if screenshot exists)
   - Satisfaction dot: 8px circle — `#4ade80` (great), `#fbbf24` (good), `#8a8070` (mixed)
   - Camera icon: small inline SVG, `var(--text-muted)`, only shown when screenshot exists

**Staggered animation:** Each card gets `animation-delay: calc(index * 0.07s)`.

### Story Modal

Reuses existing modal infrastructure (backdrop blur, close button, fadeUp animation).

```
Modal max-width: 540px
Border-radius: 24px (rounder than existing modals to match card style)
Padding: 2rem

Content:
  - Category pill + city (same as card)
  - Title (DM Serif Display, 1.3rem)
  - Full story text (DM Mono, 0.8rem, line-height 1.7)
  - Screenshot image (if exists):
      max-width: 100%
      border-radius: 16px
      object-fit: cover
      margin: 1rem 0
  - Metadata block (only fields that exist):
      Rep contacted: [name]
      Resolved in: [N] days
      Satisfaction: [dot] [label]
  - Author + date line: "-- [author] . [Month Year]"
```

### "Share Your Story" Button

Positioned below the card grid, centered.

```css
.story-cta {
  /* Follows .btn-primary pattern */
  background: var(--accent);
  color: var(--on-accent);
  border-radius: 14px;
  padding: 0.75rem 1.8rem;
  font-family: 'DM Mono', monospace;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  min-height: 44px;
}
```

Links to the Google Form in a new tab. The form URL is a placeholder (`#`) until the maintainer creates the actual Google Form and updates the link.

---

## Animation & Motion

| Element | Animation | Duration | Easing |
|---------|-----------|----------|--------|
| Card entrance | `fadeUp` (existing keyframe) | 0.4s | ease, staggered 0.07s |
| Card hover | border-color + shadow + translateY(-3px) | 0.25s | `cubic-bezier(0.16, 1, 0.3, 1)` |
| Modal open | `fadeUp` (existing) | 0.3s | ease |
| Modal backdrop | opacity 0 to 1 | 0.2s | ease |

**Reduced motion:** When `prefers-reduced-motion: reduce`, disable fadeUp animation (render instantly). Keep hover color changes but remove transform.

---

## Accessibility

| Requirement | Implementation |
|-------------|---------------|
| Keyboard navigation | Cards are `role="button"`, `tabindex="0"`, activated via Enter/Space |
| Focus visible | `outline: 2px solid var(--accent)` on `:focus-visible` |
| Modal focus trap | Focus trapped inside modal when open; returns to triggering card on close |
| Modal semantics | `role="dialog"`, `aria-modal="true"`, `aria-label="Story: [title]"` |
| Satisfaction dots | `aria-label="Satisfaction: great"` on the dot element |
| Screenshot alt text | `alt` attribute from a future JSON field, or fallback to `"Screenshot from [title]"` |
| Touch targets | Entire card is clickable (well above 44px minimum) |
| Contrast | All text uses existing CivicConnect tokens which meet 4.5:1 |
| Screen reader | Story cards announced as "[title], [category], [city]. Press Enter to read full story." |

---

## Responsive Behavior

| Breakpoint | Layout | Card padding | Section padding |
|------------|--------|-------------|----------------|
| >480px (desktop) | 2-column grid, gap 1rem | 1.5rem 1.8rem | 3rem 0 |
| <=480px (mobile) | Single column, full-width | 1.2rem 1.4rem | 2rem 0 |

Mobile adjustments:
- Category pill font: 0.6rem
- Title: 0.95rem
- Modal: full-width minus 1rem margin each side, border-radius 20px

---

## Data Loading

- Fetch `data/stories.json` on page load (same approach as leaders.json)
- Cache in a variable after first load
- If fetch fails or file is empty, hide the entire section (no error shown — just absent)
- If JSON has 0 stories, hide the section entirely

---

## File Changes

| File | Change |
|------|--------|
| `index.html` | Add stories section HTML, CSS, and JS |
| `data/stories.json` | New file — story data array |
| `data/images/stories/` | New directory — screenshot images |

No new external dependencies. No build step changes. No new API calls.

---

## Future Migration Path

When enough stories accumulate to justify a dedicated page:
1. Create `stories.html` with the same card rendering code
2. Replace inline card grid with a "Featured stories" subset (2-3 cards)
3. Add "See all stories" link pointing to `stories.html`
4. Card rendering JS and JSON structure remain identical — no throwaway work
