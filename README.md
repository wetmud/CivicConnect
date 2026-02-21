# 🍁 CivicReach

**Find every elected official who represents your address — and contact them directly.**

CivicReach is a free, open civic tool for Canadians. Enter your address and instantly see who represents you at every level of government — from your city councillor up to your MPP. Write to them directly with AI-assisted email drafting. No account. No login. No data collected. Ever.

---

## Features

- **Address autocomplete** — powered by Geoapify, Canadian addresses only
- **Representatives tab** — finds your mayor, city councillor, regional rep, and MPP sorted from most local to provincial
- **Nearby Services tab** — shows libraries, parks, hospitals, and schools within your area with distance from your address
- **Ward map** — displays your ward or district boundary on an interactive map
- **AI email drafting** — select an issue, describe it, and get a polished email drafted for you ready to send
- **Light / Dark mode** — toggle between themes
- **Zero data collection** — everything runs in your browser, nothing is stored or logged

---

## Tech Stack

| Layer | Tool |
|---|---|
| Frontend | Vanilla HTML / CSS / JavaScript — single file, no framework |
| Address autocomplete & geocoding | [Geoapify API](https://www.geoapify.com) |
| Representatives data | [Represent API](https://represent.opennorth.ca) by OpenNorth (free, open data) |
| Ward boundaries | Represent API boundaries endpoint + GeoJSON |
| Map | [Leaflet.js](https://leafletjs.com) + CartoDB tiles |
| AI email drafting | Anthropic Claude API |
| CORS proxy | [corsproxy.io](https://corsproxy.io) (temporary — replace with own proxy before production) |

---

## Getting Started

This project is a single HTML file. No build step, no dependencies to install.

1. Clone or download the repo
2. Open `civic-contact.html` in any browser
3. Start searching

### API Keys

You will need a free [Geoapify API key](https://myprojects.geoapify.com). Replace the key in the HTML:

```js
const GEOAPIFY_KEY = 'your_key_here';
```

The Represent API is free and requires no key.

The AI email drafting feature requires an [Anthropic API key](https://console.anthropic.com). This should **not** be hardcoded in the frontend — see the security section below.

---

## Deployment

### Free testing (recommended starting point)

1. Push this repo to GitHub
2. Go to **Settings → Pages** and set the source to your main branch
3. Your site will be live at `https://yourusername.github.io/civicreach` within minutes

### Production setup

For production, replace the hardcoded Geoapify key and the public CORS proxy with a lightweight backend proxy. Recommended options:

- **Cloudflare Workers** — free tier covers 100,000 requests/day, global edge network
- **Vercel serverless functions** — also free to start

The proxy holds your API keys server-side and forwards requests from the frontend. This prevents key exposure and allows rate limiting.

---

## Security

- **Content Security Policy** is set via meta tag — only whitelisted domains can load resources
- **No-referrer policy** prevents address data from leaking in HTTP headers
- **No cookies, no localStorage, no sessions** — nothing persists between visits
- **No analytics or tracking scripts** of any kind

### Before going to production

- [ ] Move Geoapify key to a backend proxy (Cloudflare Worker or Vercel function)
- [ ] Replace `corsproxy.io` with your own proxy
- [ ] Add per-IP rate limiting on your proxy
- [ ] Point a custom domain at GitHub Pages
- [ ] Review Anthropic API key handling

---

## Privacy

CivicReach is built on a simple promise: **we collect nothing**.

Addresses searched, representatives viewed, emails drafted — none of it is stored, logged, or transmitted to any server we control. Every operation happens in the user's browser and disappears when they leave.

This is not just an ethical choice — it is a technical one. There is no database, no backend, no user accounts. There is nothing to breach.

---

## Data Sources

| Data | Source | License |
|---|---|---|
| Canadian representatives | [OpenNorth Represent API](https://represent.opennorth.ca) | Open Data |
| Ward / district boundaries | [OpenNorth Represent API](https://represent.opennorth.ca) | Open Data |
| Address geocoding | [Geoapify](https://www.geoapify.com) | Free tier |
| Nearby places | [Geoapify Places API](https://www.geoapify.com/places-api) | Free tier |
| Map tiles | [CartoDB](https://carto.com/basemaps) via Leaflet | Free / attribution required |

---

## Roadmap

- [ ] Federal representatives tab (code already saved in comments, ready to enable)
- [ ] US address support via Google Civic Information API
- [ ] Cloudflare Worker proxy for API key security
- [ ] Custom domain (civicreach.ca)
- [ ] Mobile layout polish
- [ ] Share a rep's contact info via link
- [ ] Public issue tracker — anonymously surfaced local concerns by neighbourhood

---

## Contributing

This project is in early development. If you find a bug, have a feature idea, or want to contribute, open an issue or pull request.

---

## License

MIT — free to use, modify, and distribute with attribution.

---

*Built in Burlington, Ontario 🍁 — for Canadians who want to actually use their democracy.*
