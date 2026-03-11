# Design: Wikidata Social Links, Modal Email Display, Search Bar Polish
**Date:** 2026-03-11

## Scope
Three focused improvements to `index.html`:
1. Auto-populate rep social links via Wikidata API
2. Show clickable/copyable email address in rep profile modal
3. Pill-shaped search bar with shadow

---

## 1. Wikidata Social Links

**Goal:** Fill social media gaps that Represent API leaves empty.

**Approach:** On modal open, fire `fetchWikidataSocials(rep)` in parallel with the existing `fetchWikiBio(rep)`. Query the Wikidata API (action=wbsearchentities + wbgetentities) by rep name. Extract social handles from structured properties:
- P2002 → Twitter/X
- P2003 → Instagram
- P2013 → Facebook
- P2397 → YouTube
- P6634 → LinkedIn

**Merge logic:** Represent API data takes priority. Wikidata fills only empty slots.

**Caching:** `wikidataCache` object keyed by `rep.name` — same pattern as `wikiCache`. Prevents redundant API calls if modal is reopened.

**Error handling:** Silent fail — if Wikidata returns no match or network error, social row shows only what Represent provided (current behaviour).

**After fetch:** Call a shared `renderModalSocials(rep, wikidataExtra)` helper that merges both sources and updates `#rpModal-social`.

---

## 2. Email Display in Modal

**Goal:** Show the rep's full email address in the modal, clickable to copy.

**Placement:** Left column, below `#rpModal-party`, above `#rpModal-social`.

**Element:** `<div id="rpModal-email-display" class="rep-profile-email">` — hidden via `display:none` when no email.

**Interaction:** Click → copy to clipboard → text flashes "Copied ✓" for 1.5s, then reverts to email address. Uses `navigator.clipboard.writeText()`.

**Styling:** Monospace font, muted color, small size, subtle hover underline. No button chrome — looks like a plain address that invites clicking.

---

## 3. Search Bar: Pill Shape + Shadow

**Changes to `.input-row`:**
- `border-radius: 4px` → `border-radius: 2rem`
- Add `box-shadow: 0 2px 12px rgba(0,0,0,0.10)` (dark mode) / lighter equivalent (light mode)

**`#search-btn`:**
- Add `border-radius: 0 2rem 2rem 0` to follow the pill curve on the right side

**Focus state:** Existing `border-color: var(--accent)` stays; shadow intensifies slightly on `:focus-within`.

---

## Files Changed
- `index.html` only (single-file architecture)
