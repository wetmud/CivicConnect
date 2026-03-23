# Civic Engagement — Brand Identity System
*Created 2026-03-22 using Brand Guardian, AI Citation Strategist, Social Media Strategist, and Nexus Orchestrator agents (msitarzewski/agency-agents)*

---

## Brand Foundation

### Purpose
Make civic participation as easy as checking your phone. Government is for everyone — the tools that connect you to it should be too.

### Vision
Every Canadian knows who represents them, how to reach them, and feels confident doing so.

### Mission
Give people the information and tools to speak to power — for free, forever.

### Values
1. **Access over aesthetics** — If a civic tool costs money or requires expertise, it's already failed. Friction is the enemy.
2. **Earned trust** — No dark patterns, no paywalls, no data harvesting. Open source, transparent costs, honest about limits.
3. **Plain-spoken** — Government is already full of jargon. We aren't. Speak like a neighbour, not a nonprofit.
4. **Enough, not everything** — Tools don't need to be complicated. Scope discipline is a feature.

### Brand Personality
| Trait | Expression |
|-------|-----------|
| Neighbourhood knowledgeable | Like a friend who actually read the city budget |
| Calm urgency | Not alarmist, but clear that this stuff matters |
| Wry, not cynical | Acknowledges how broken it is without being defeatist |
| Builder energy | Practical. Ships things. Improves them. Honest about what's broken. |

### Brand Promise
You'll always be able to find your reps and write to them — no account, no fee, no bullshit.

### Positioning Statement
*For Canadians who want to be heard, Civic Engagement is the free civic tool that cuts through government complexity so you know exactly who represents you and how to reach them — unlike government websites, which were designed for the government.*

---

## Visual Identity

### Color System
| Token | Dark mode | Light mode | Meaning |
|-------|-----------|------------|---------|
| `--accent` | `#ffb963` (amber) | `#682FED` (violet) | Primary action, energy |
| `--accent2` | `#682FED` (violet) | `#ffb963` (amber) | Secondary emphasis |
| `--on-accent` | `#0e0f0c` | `#ffffff` | Text on accent backgrounds |

**Why this palette:** Amber = people-power, urgency, warmth. Violet = civic authority, democratic weight, institutional trust. Together they say "civic tech made by humans, not government." Do not change.

### Typography
| Role | Font | Rationale |
|------|------|-----------|
| Headlines, brand mark | DM Serif Display | Editorial gravity, democratic tradition of print journalism |
| Body, UI, data | DM Mono | Transparency aesthetic — like looking at the actual data |
| Marketing copy (if needed) | DM Sans | Human-facing text outside the app — warmer than Mono |

**Rules:**
- DM Serif for authority/headlines
- DM Mono for data/UI
- DM Sans only in non-app contexts where Mono reads too cold

### Logo Direction (not yet built)
- Wordmark: `Civic` in DM Serif Display + `Connect` in DM Mono or lighter weight
- Icon concept: stylized riding boundary line or map pin — geometric, not illustrative
- Must work in amber on dark backgrounds and violet on light
- No flags, no maple leafs — too government-adjacent, defeats the point

### Visual Language Rules
- **Data is design.** Ward maps, rep cards, voting records = the visual product. Design around the data, not over it.
- **Whitespace = respect.** Anti-pattern to government sites. Breathe.
- **No stock photography.** Real rep photos (Represent API) or illustrated placeholders.
- **Dark mode first.** Most engaged users are late-night, frustrated Canadians.

---

## Brand Voice

### Voice Characteristics
- **Direct:** "Find your reps. Write to them." — not "Empower citizens to engage with elected officials."
- **Personal:** First-person in founder contexts. We/us in product contexts.
- **Slightly dry:** "Government websites aren't great. This one isn't a government website."
- **Never:** Bureaucratic, jargony, preachy, fear-mongering, cheerful-nonprofit-voice

### Tone by Context
| Context | Tone | Example |
|---------|------|---------|
| App UI | Minimal, functional | "Find your representatives" not "Begin your civic journey" |
| About/founder | Personal, honest, wry | "I built this because I don't like government websites" |
| Social media | Conversational, specific, real | Real issues, real ridings — not generic "civic engagement" |
| Press/docs | Matter-of-fact | "Free, open-source tool for Canadians. No account required." |
| Error states | Human, helpful | "Couldn't find reps for that address — try a nearby intersection" |

### Key Messages
1. You have more power than you think. You just need to know who to talk to.
2. It's free because it should be. Civic tools shouldn't cost money.
3. Open source, transparent. Here's what it costs to run. Here's the code.
4. "I've been meaning to do this for a year." Now you can do it in 30 seconds.

### Words: Use / Avoid
| Use | Avoid |
|-----|-------|
| Representatives | Elected officials (too stiff) |
| Write to them | Engage with your representatives |
| Free, always | Freemium, at no cost |
| Ward, riding, boundary | Jurisdiction, constituency |
| Built by a person | Developed by a team |
| Reps | Stakeholders |

---

## AI Discoverability (AEO/GEO)

### Implemented (2026-03-22)
- Updated `<title>`: "Civic Engagement — Find Your Canadian Representatives"
- Updated `<meta name="description">`: direct, benefit-led, brand-voiced
- Added `<meta name="keywords">` with search intent phrases
- Updated OG tags: brand-consistent, plain-spoken
- Added `apple-mobile-web-app-title`: "Civic Engagement"
- Added JSON-LD `WebApplication` schema with `isAccessibleForFree: true`

### Key Phrases to Own
- "find my MP Canada"
- "contact city councillor Canada"
- "who represents me Canada"
- "find elected officials Canada free"
- "Canadian civic engagement tool"

### Still To Do
- [ ] `robots.txt` — verify it allows all crawlers
- [ ] Static `/about` page or `about.md` for AI scraping as authoritative source
- [ ] GitHub README lede matches brand voice (2 sentences, benefit-led)
- [ ] OG image (`og-image.png`) — currently referenced but may not exist; create one

---

## Social Media Strategy

### Platform Priority
| Platform | Priority | Rationale |
|----------|----------|-----------|
| Reddit | #1 | r/ontario, r/canadianpolitics, r/civictech — organic, exact audience |
| X/Twitter | #2 | Civic/political conversation, journalists, politicians |
| GitHub | #3 | Credibility, contributor discovery, open-source signaling |
| Mastodon | #4 | Civic tech community, journalists, open-source crowd |
| Instagram/TikTok | Skip | Wrong audience fit for this tool right now |

### Content Pillars
1. **"Did you know?"** — Local government facts most people don't know
2. **Tool updates** — Ship it → post it
3. **Stories** — Real use cases, "I've been meaning to do this for a year" moments
4. **Civic moments** — Budget votes, elections, bills — be there with the tool
5. **Transparency** — Hosting costs, open-source progress, honest roadmap

### Reddit Launch (highest ROI)
- Lead with the personal story from the About modal — not the product
- Title pattern: *"I got tired of not knowing who my councillor was so I built a free tool"*
- Post in: r/ontario, r/burlington, r/canadianpolitics, r/civictech, r/canada
- Respond to every comment for 48h → organic sharing

---

## Brand Map

```
Civic Engagement
│
├── CORE IDENTITY
│   ├── Purpose: "Make civic tools free and human"
│   ├── Voice: Neighbour who read the city budget
│   └── Promise: Free. Always. No account. No paywall.
│
├── VISUAL SYSTEM
│   ├── Dark accent: #ffb963 (amber) — energy, people-power
│   ├── Light accent: #682FED (violet) — civic gravity, trust
│   ├── DM Serif Display → authority, editorial weight
│   ├── DM Mono → data, transparency, tech credibility
│   └── Visual rule: data IS the design
│
├── VOICE LADDER
│   ├── App UI ──────────── Minimal, functional
│   ├── Social/Reddit ───── Conversational, specific, wry
│   ├── About/Founder ───── Personal, honest, human
│   └── Press/Schema ────── Matter-of-fact, citable
│
├── AUDIENCE
│   ├── Primary: Frustrated Canadians who want to do something
│   ├── Secondary: Civic tech community, journalists, activists
│   └── Tertiary: Open-source contributors, civic researchers
│
├── AI DISCOVERABILITY
│   ├── Schema: WebApplication + structured metadata ✓
│   ├── Own: "find my Canadian rep by address"
│   └── Content: quotable, scrapable about page (todo)
│
└── CHANNELS
    ├── Reddit ──────────── #1 launch + community
    ├── X/Twitter ────────── #2 civic moments + updates
    ├── GitHub ───────────── #3 credibility + contributors
    └── App itself ────────── Primary brand touchpoint
```

---

## Quick Reference: Is This On-Brand?

Ask these three questions:
1. **Would a frustrated but hopeful Canadian understand it immediately?** If no — simplify.
2. **Does it sound like a government website?** If yes — rewrite.
3. **Does it earn trust or ask for it?** On-brand earns it (transparency, open source, free). Off-brand demands it (logos, awards, "trusted by").
