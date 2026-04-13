# Success Stories Section — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a curated "Real Results" success stories section to CivicConnect where visitors see how real people used the tool to get results from their elected officials.

**Architecture:** Stories are stored as a JSON array in `data/stories.json`, fetched at runtime (same pattern as `data/leaders.json`). Cards render in a standalone section above the footer. Clicking a card opens a detail modal reusing the existing `about-backdrop`/`about-box` modal pattern. All code lives in `index.html` (CSS + HTML + JS). No new dependencies.

**Tech Stack:** Vanilla HTML/CSS/JS, single-file architecture, GitHub Pages deployment.

**Spec:** `docs/superpowers/specs/2026-04-12-success-stories-design.md`

**Security note:** All dynamic values from `stories.json` are sanitized through `escHtml()` and `escAttr()` before DOM insertion — the same pattern used by every other card renderer in this codebase (reps, leaders, services, Civil chat). The data source is a maintainer-curated JSON file committed to the repo, not untrusted user input.

---

## File Map

| File | Action | Purpose |
|------|--------|---------|
| `index.html` ~line 2469 | Modify (CSS) | Add story card, section, and modal styles |
| `index.html` ~line 2849 | Modify (HTML) | Add stories section + modal markup before `<footer>` |
| `index.html` ~line 5401 | Modify (JS) | Add stories fetch, render, and modal functions |
| `data/stories.json` | Create | Story data array (starts with 1-2 seed stories) |
| `data/images/stories/` | Create | Directory for screenshot images |

---

### Task 1: Create the story data file and images directory

**Files:**
- Create: `data/stories.json`
- Create: `data/images/stories/.gitkeep`

- [ ] **Step 1: Create `data/stories.json` with two seed stories**

```json
[
  {
    "id": "pothole-burlington-2026",
    "title": "Pothole Fixed in 48 Hours",
    "text": "After using CivicEngage to find my councillor, I sent one email describing the pothole on Brant Street. Got a call back the next day, crew was out within 48 hours. This tool made it so easy to find who to contact.",
    "category": "Infrastructure",
    "city": "Burlington, ON",
    "rep_contacted": "Lisa Kearns",
    "days_to_resolve": 2,
    "satisfaction": "great",
    "date": "2026-03-15",
    "author": "Anonymous"
  },
  {
    "id": "noise-bylaw-toronto-2026",
    "title": "Noise Bylaw Complaint Resolved",
    "text": "Construction noise was starting at 6am in my neighbourhood. Found my city councillor through CivicEngage, sent a detailed email, and bylaw enforcement visited the site within a week.",
    "category": "Services",
    "city": "Toronto, ON",
    "days_to_resolve": 7,
    "satisfaction": "great",
    "date": "2026-02-20",
    "author": "M.R."
  }
]
```

These are seed/placeholder stories. Replace with real submissions when they come in. Neither has a `screenshot` field — that's intentional (optional field).

- [ ] **Step 2: Create the images directory with a `.gitkeep`**

```bash
mkdir -p data/images/stories
touch data/images/stories/.gitkeep
```

- [ ] **Step 3: Verify the JSON is valid**

```bash
python3 -c "import json; json.load(open('data/stories.json')); print('Valid JSON')"
```

Expected: `Valid JSON`

- [ ] **Step 4: Commit**

```bash
git add data/stories.json data/images/stories/.gitkeep
git commit -m "Add stories data file and images directory"
```

---

### Task 2: Add CSS styles for the stories section

**Files:**
- Modify: `index.html` — insert CSS before the closing `</style>` tag (~line 2470)

The CSS goes right before `</style>` on line 2470. Insert after the existing Civil AI styles (after the `@media (max-width: 400px)` block ending on ~line 2468).

- [ ] **Step 1: Add stories section CSS**

Insert the following CSS block immediately before `  </style>` (line 2470):

```css

    /* ── Stories section ─────────────────────────────── */
    .stories-section {
      max-width: 680px;
      margin: 0 auto;
      padding: 3rem 1rem;
    }
    .stories-section h2 {
      font-family: 'DM Serif Display', serif;
      font-size: clamp(1.6rem, 4vw, 2.2rem);
      margin-bottom: 0.25rem;
    }
    .stories-subtitle {
      font-family: 'DM Mono', monospace;
      font-size: 0.75rem;
      color: var(--text-muted);
      margin-bottom: 1.5rem;
    }
    .stories-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1rem;
      margin-bottom: 1.5rem;
    }
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
    .story-card:hover, .story-card:focus-visible {
      border-color: var(--accent);
      box-shadow: 0 8px 28px rgba(0, 0, 0, 0.16);
      transform: translateY(-3px);
    }
    .story-card:focus-visible {
      outline: 2px solid var(--accent);
      outline-offset: 2px;
    }
    .story-meta {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      margin-bottom: 0.5rem;
      flex-wrap: wrap;
    }
    .story-category {
      font-family: 'DM Mono', monospace;
      font-size: 0.6rem;
      text-transform: uppercase;
      letter-spacing: 0.06em;
      color: var(--text-muted);
      background: var(--border);
      border-radius: 12px;
      padding: 0.15rem 0.6rem;
    }
    .story-city {
      font-family: 'DM Mono', monospace;
      font-size: 0.6rem;
      color: var(--text-muted);
    }
    .story-title {
      font-family: 'DM Serif Display', serif;
      font-size: 1rem;
      margin-bottom: 0.4rem;
      color: var(--text);
    }
    .story-excerpt {
      font-family: 'DM Mono', monospace;
      font-size: 0.72rem;
      color: var(--text-muted);
      line-height: 1.6;
      margin-bottom: 0.6rem;
    }
    .story-footer {
      display: flex;
      align-items: center;
      gap: 0.6rem;
      font-family: 'DM Mono', monospace;
      font-size: 0.6rem;
      color: var(--text-muted);
      flex-wrap: wrap;
    }
    .story-dot {
      display: inline-block;
      width: 8px; height: 8px;
      border-radius: 50%;
      vertical-align: middle;
    }
    .story-dot.great { background: #4ade80; }
    .story-dot.good  { background: #fbbf24; }
    .story-dot.mixed { background: #8a8070; }
    .story-camera {
      width: 14px; height: 14px;
      vertical-align: middle;
      opacity: 0.5;
    }
    .stories-actions {
      text-align: center;
      margin-top: 0.5rem;
    }
    .story-cta {
      display: inline-block;
      background: var(--accent);
      color: var(--on-accent);
      border: none;
      border-radius: 14px;
      padding: 0.75rem 1.8rem;
      font-family: 'DM Mono', monospace;
      font-size: 0.75rem;
      text-transform: uppercase;
      letter-spacing: 0.08em;
      text-decoration: none;
      min-height: 44px;
      cursor: pointer;
      transition: opacity 0.2s ease;
    }
    .story-cta:hover { opacity: 0.85; }
    .stories-toggle {
      display: block;
      margin: 1rem auto 0;
      background: none;
      border: none;
      color: var(--accent);
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      cursor: pointer;
      text-decoration: underline;
      text-underline-offset: 3px;
    }
    .stories-toggle:hover { opacity: 0.7; }

    /* Story modal overrides */
    .story-modal-box {
      max-width: 540px;
      border-radius: 24px;
    }
    .story-modal-box .story-title {
      font-size: 1.3rem;
      margin-bottom: 0.6rem;
    }
    .story-modal-text {
      font-family: 'DM Mono', monospace;
      font-size: 0.78rem;
      line-height: 1.7;
      color: var(--text);
      margin-bottom: 1rem;
    }
    .story-modal-img {
      width: 100%;
      border-radius: 16px;
      object-fit: cover;
      margin: 0.75rem 0;
    }
    .story-modal-meta {
      font-family: 'DM Mono', monospace;
      font-size: 0.68rem;
      color: var(--text-muted);
      line-height: 1.8;
    }
    .story-modal-author {
      font-family: 'DM Mono', monospace;
      font-size: 0.68rem;
      color: var(--text-muted);
      margin-top: 0.75rem;
      font-style: italic;
    }

    @media (max-width: 480px) {
      .stories-section { padding: 2rem 1rem; }
      .stories-grid { grid-template-columns: 1fr; }
      .story-card { padding: 1.2rem 1.4rem; }
      .story-category { font-size: 0.55rem; }
      .story-title { font-size: 0.95rem; }
      .story-modal-box { border-radius: 20px; }
    }
    @media (prefers-reduced-motion: reduce) {
      .story-card { animation: none; }
      .story-card:hover { transform: none; }
    }
```

- [ ] **Step 2: Verify — open `index.html` in a browser**

Check that the page still loads without CSS errors. The stories section won't be visible yet (no HTML), but no existing styles should break.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add CSS styles for success stories section"
```

---

### Task 3: Add HTML markup for the stories section and modal

**Files:**
- Modify: `index.html` — insert HTML before `<footer>` (~line 2850)

The HTML goes immediately before `<footer>` (line 2850), after the Tips Modal closing `</div>` (line 2848).

- [ ] **Step 1: Add stories section HTML and modal**

Insert the following HTML block between the blank line after the Tips modal (after line 2849) and before `<footer>` (line 2850):

```html
<!-- ── Success Stories ───────────────────────────────── -->
<section class="stories-section" id="stories-section" style="display:none;">
  <h2>Real <em>Results</em></h2>
  <p class="stories-subtitle">People who took action — and got heard.</p>
  <div class="stories-grid" id="stories-grid"></div>
  <div class="stories-actions">
    <a class="story-cta" href="#" target="_blank" rel="noopener">Share Your Story &rarr;</a>
  </div>
</section>

<!-- Story Detail Modal -->
<div class="about-backdrop" id="story-backdrop" onclick="closeStoryModal(event)">
  <div class="about-box story-modal-box" role="dialog" aria-modal="true" aria-label="Story detail">
    <button class="about-close" onclick="closeStoryModal()">&#10005;</button>
    <div id="story-modal-content"></div>
  </div>
</div>

```

Key details:
- Section starts with `display:none` — JS shows it only when stories load successfully
- `stories-grid` is empty — JS populates it
- Modal reuses `about-backdrop` and `about-box` classes (existing modal infrastructure), with `story-modal-box` for overrides (wider border-radius)
- `story-modal-content` is populated dynamically when a card is clicked. All values are sanitized via `escHtml()` / `escAttr()` before insertion — same pattern as rep cards, leader cards, and Civil chat.
- CTA button `href="#"` is a placeholder — replace with real Google Form URL when created

- [ ] **Step 2: Verify — open `index.html` in a browser**

The section should be hidden (display:none). The modal shouldn't be visible. No layout changes to existing page. Check that the footer still appears correctly.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add stories section and modal HTML markup"
```

---

### Task 4: Add JavaScript for fetching, rendering, and modal interaction

**Files:**
- Modify: `index.html` — insert JS before `</script>` (~line 5402)

Insert the following JavaScript block before `</script>` (line 5402), after the Civil AI `DOMContentLoaded` listener block (after line 5400).

All dynamic content from `stories.json` is sanitized through `escHtml()` (line 3684) and `escAttr()` (line 3674) before insertion — the same security pattern used by `renderRepList()`, `buildLeaderCard()`, and every other renderer in this file.

- [ ] **Step 1: Add the stories JavaScript**

Insert the following JS block immediately before `</script>`:

```javascript

// ── Success Stories ──────────────────────────────────────
let _storiesData = null;
const STORIES_VISIBLE_MAX = 5;

async function loadStories() {
  try {
    const resp = await fetch('data/stories.json');
    if (!resp.ok) return;
    const stories = await resp.json();
    if (!Array.isArray(stories) || stories.length === 0) return;
    _storiesData = stories;
    renderStories(stories);
    document.getElementById('stories-section').style.display = '';
  } catch (e) {
    // Silently hide section if stories fail to load
  }
}

function renderStories(stories) {
  const grid = document.getElementById('stories-grid');
  grid.innerHTML = '';
  stories.forEach((story, i) => {
    const card = document.createElement('div');
    card.className = 'story-card';
    card.setAttribute('role', 'button');
    card.setAttribute('tabindex', '0');
    card.setAttribute('aria-label', buildStoryAriaLabel(story));
    card.style.animationDelay = (i * 0.07) + 's';
    if (i >= STORIES_VISIBLE_MAX) card.style.display = 'none';
    card.onclick = () => openStoryModal(story);
    card.onkeydown = (e) => { if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); openStoryModal(story); } };

    let metaHtml = '';
    if (story.category) metaHtml += '<span class="story-category">' + escHtml(story.category) + '</span>';
    if (story.city) metaHtml += '<span class="story-city">' + escHtml(story.city) + '</span>';

    const excerpt = story.text.length > 120 ? escHtml(story.text.slice(0, 120)) + '&hellip;' : escHtml(story.text);

    let footerParts = [];
    if (story.satisfaction) {
      footerParts.push('<span class="story-dot ' + escAttr(story.satisfaction) + '" aria-label="Satisfaction: ' + escAttr(story.satisfaction) + '"></span> ' + escHtml(story.satisfaction));
    }
    if (story.days_to_resolve != null) {
      footerParts.push(story.days_to_resolve + (story.days_to_resolve === 1 ? ' day' : ' days'));
    }
    if (story.date) footerParts.push(formatStoryDate(story.date));
    if (story.screenshot) {
      footerParts.push('<svg class="story-camera" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/></svg>');
    }

    card.innerHTML =
      (metaHtml ? '<div class="story-meta">' + metaHtml + '</div>' : '') +
      '<div class="story-title">' + escHtml(story.title) + '</div>' +
      '<div class="story-excerpt">' + excerpt + '</div>' +
      (footerParts.length ? '<div class="story-footer">' + footerParts.join(' <span style="opacity:0.4;">&middot;</span> ') + '</div>' : '');

    grid.appendChild(card);
  });

  // "See all" toggle if more than STORIES_VISIBLE_MAX
  if (stories.length > STORIES_VISIBLE_MAX) {
    const existing = document.querySelector('.stories-toggle');
    if (existing) existing.remove();
    const btn = document.createElement('button');
    btn.className = 'stories-toggle';
    btn.textContent = 'See all ' + stories.length + ' stories \u2192';
    btn.onclick = () => toggleAllStories(btn);
    grid.parentElement.insertBefore(btn, grid.nextSibling);
  }
}

function toggleAllStories(btn) {
  const grid = document.getElementById('stories-grid');
  const hidden = grid.querySelectorAll('.story-card[style*="display: none"]');
  if (hidden.length > 0) {
    hidden.forEach(c => { c.style.display = ''; });
    btn.textContent = 'Show fewer \u2190';
  } else {
    const cards = grid.querySelectorAll('.story-card');
    cards.forEach((c, i) => { if (i >= STORIES_VISIBLE_MAX) c.style.display = 'none'; });
    btn.textContent = 'See all ' + _storiesData.length + ' stories \u2192';
  }
}

function openStoryModal(story) {
  const el = document.getElementById('story-modal-content');
  const backdrop = document.getElementById('story-backdrop');

  let metaHtml = '';
  if (story.category) metaHtml += '<span class="story-category">' + escHtml(story.category) + '</span>';
  if (story.city) metaHtml += '<span class="story-city">' + escHtml(story.city) + '</span>';

  let html = '';
  if (metaHtml) html += '<div class="story-meta">' + metaHtml + '</div>';
  html += '<div class="story-title">' + escHtml(story.title) + '</div>';
  html += '<div class="story-modal-text">' + escHtml(story.text) + '</div>';

  if (story.screenshot) {
    html += '<img class="story-modal-img" src="data/' + escAttr(story.screenshot) + '" alt="Screenshot from ' + escAttr(story.title) + '" loading="lazy">';
  }

  let metaLines = [];
  if (story.rep_contacted) metaLines.push('Rep contacted: ' + escHtml(story.rep_contacted));
  if (story.days_to_resolve != null) metaLines.push('Resolved in: ' + story.days_to_resolve + (story.days_to_resolve === 1 ? ' day' : ' days'));
  if (story.satisfaction) metaLines.push('Satisfaction: <span class="story-dot ' + escAttr(story.satisfaction) + '"></span> ' + escHtml(story.satisfaction));
  if (metaLines.length) html += '<div class="story-modal-meta">' + metaLines.join('<br>') + '</div>';

  const authorName = story.author || 'Anonymous';
  const dateStr = story.date ? formatStoryDate(story.date) : '';
  html += '<div class="story-modal-author">&mdash; ' + escHtml(authorName) + (dateStr ? ' &middot; ' + dateStr : '') + '</div>';

  el.innerHTML = html;
  backdrop.querySelector('.about-box').setAttribute('aria-label', 'Story: ' + story.title);
  backdrop.classList.add('open');
}

function closeStoryModal(e) {
  if (!e || e.target === document.getElementById('story-backdrop')) {
    document.getElementById('story-backdrop').classList.remove('open');
  }
}

function formatStoryDate(dateStr) {
  try {
    const d = new Date(dateStr + 'T00:00:00');
    return d.toLocaleDateString('en-CA', { month: 'short', year: 'numeric' });
  } catch (e) {
    return dateStr;
  }
}

function buildStoryAriaLabel(story) {
  let label = story.title;
  if (story.category) label += ', ' + story.category;
  if (story.city) label += ', ' + story.city;
  label += '. Press Enter to read full story.';
  return label;
}

// Load stories on page ready
document.addEventListener('DOMContentLoaded', loadStories);
```

- [ ] **Step 2: Verify in browser — full test**

Open `index.html` in a browser and check:

1. **Section visible:** "Real Results" section appears above the footer with 2 seed story cards
2. **Card layout:** 2-column grid on desktop, single column on mobile (resize to ≤480px)
3. **Card content:** Each card shows category pill, city, title, excerpt (truncated), satisfaction dot, resolution time, date
4. **Card hover:** Border glows accent color, shadow deepens, card lifts up
5. **Card keyboard:** Tab to card, press Enter — modal opens
6. **Modal opens:** Click a card — modal appears with backdrop blur, full story text, metadata, author line
7. **Modal closes:** Click backdrop or X button — modal closes
8. **No screenshot field:** Neither seed story has a screenshot, so no camera icon on cards and no image in modal — verify both are absent
9. **CTA button:** "Share Your Story" button visible, styled with accent color, links to `#`
10. **No "See all" toggle:** Only 2 stories, below the 5-card threshold — toggle button should not appear
11. **Dark/light mode:** Toggle theme — section should adapt via CSS custom properties
12. **Reduced motion:** In browser devtools, enable `prefers-reduced-motion: reduce` — cards should appear without animation, hover should not translate

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add stories fetch, render, and modal JavaScript"
```

---

### Task 5: Final verification and cleanup

**Files:**
- Review: `index.html`, `data/stories.json`

- [ ] **Step 1: Verify no console errors**

Open browser devtools Console tab. Reload the page. Check for zero errors or warnings related to stories.

- [ ] **Step 2: Verify stories.json loads from correct path**

In devtools Network tab, reload and confirm `data/stories.json` returns 200 with correct JSON content.

- [ ] **Step 3: Test empty state — temporarily empty the stories array**

Edit `data/stories.json` to `[]`, reload. The stories section should be completely hidden (not visible at all). Then restore the seed stories.

- [ ] **Step 4: Test the "See all" toggle — add more seed stories temporarily**

Duplicate the 2 seed stories in `data/stories.json` to have 6+ entries (change the `id` fields to be unique). Reload and verify:
- Only 5 cards visible initially
- "See all 6 stories" toggle appears
- Clicking it reveals the 6th card
- Clicking "Show fewer" hides it again

Then remove the duplicates and restore the original 2 stories.

- [ ] **Step 5: Final commit (if any changes)**

```bash
git add -A
git commit -m "Success stories section complete"
```
