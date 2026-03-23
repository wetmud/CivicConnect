# Mobile UI Audit — civicengagement.ca
**Date:** 2026-03-23
**Agents:** UX Researcher · UI Designer · Frontend Developer · Agents Orchestrator
**Scope:** Mobile landing page (primary) + results view (secondary)

---

## Already Shipped (commit c423b0d)
- Header: collapsed About/Tips/Support into `···` dropdown on ≤600px
- Footer: removed redundant "Recommend a Feature" button

## Pending Commit (Frontend Developer changes — unstaged)
- Search button: `btn-long`/`btn-short` spans → shows "Search" on ≤480px (fixes clipping)
- Tabs: `flex-wrap: nowrap` + `data-short` "Local" / "Services" (fixes 2-row wrap)
- Rep actions: `flex-direction: column` → `grid 1fr 1fr` 2×2 layout (cuts card height ~40%)

---

## Landing Page Polish — To Implement

### 1. Hero visual hierarchy
**Issue:** "Your address." and "Your voice." are the same visual weight. "Your voice." is the emotional hook and should dominate.
**Fix:** Reduce "Your address." to a lighter weight or smaller size; let "Your voice." be the bigger, bolder line.

### 2. Hint text hidden by keyboard
**Issue:** "Start typing your Canadian address and select from suggestions." sits below the button. When keyboard opens, it's hidden behind it — exactly when users need it.
**Fix:** Move content into the input `placeholder` attribute, or reposition above the input.

### 3. Search bar border contrast (light mode)
**Issue:** Light mode `--bg` is `#faf8f5` and the input border is faint — the search pill doesn't visually pop at rest.
**Fix:** Increase border to `1.5px solid var(--border)` + add subtle box-shadow: `0 1px 4px rgba(0,0,0,0.06)`.

### 4. Subtitle line length
**Issue:** DM Mono subtitle runs wide on mobile, feels dense.
**Fix:** Cap at `max-width: 30ch; margin: 0 auto` on mobile.

---

## Lower Priority (defer)
- Focus ring: soften from solid to `rgba` glow (cosmetic)
- Hero subtitle: consider `text-align: left` on mobile if >2 lines
- Card internal padding: tighten from ~1.25rem to ~1rem if cards still feel heavy after 2×2 grid
