# Kartel Creative Portfolio

A branded video content portal for Kartel Studios. Self-contained HTML files for the internal platform, collection builder, client-facing viewer, and industry-specific portfolio pages. All powered by Google Drive video embeds and localStorage for shared data.

## Tech Stack
- Vanilla HTML/CSS/JS (no build system)
- Google Drive for video hosting/thumbnails/playback
- localStorage for collection data (bridge between internal and external pages)
- URL parameter routing for industry verticals and company personalization
- Fonts: Outfit + JetBrains Mono (Google Fonts)
- Planned migration: Next.js 14 + Vercel + Google Sheets API

## File Map

### Core HTML Files
| File | Purpose | Audience |
|------|---------|----------|
| `kartel-creative-v2.html` | Full 127-video catalog, client collections panel, industry portfolios panel, admin config, drag-and-drop | Internal (Kartel team) |
| `kartel-build.html` | 5-step wizard to create new client collections with auto-recommendation | Internal (Kartel team) |
| `kartel-view.html` | Client-facing private collection with intro video splash, password gate | External (clients) |
| `kartel-industry.html` | Unified industry portfolio page serving all 5 verticals via URL params, with company personalization | External (prospects via email outreach) |

### Support Files
| File | Purpose |
|------|---------|
| `kartel-intro.mp4` | H.264 intro video for splash screen (converted from .mov) |
| `kartel-collection-enrichment-prompt.md` | Portable prompt — paste into any Claude session with Kartel MCP tools to auto-enrich a client collection |
| `Kartel_Content_Portal_AI_Integration_Guide.docx` | Team guide mapping all 8 AI connectors to the enrichment pipeline |
| `Kartel_Industry_Email_Templates copy.docx` | 5 industry email templates (Advertising, Technology, Entertainment, Consumer, Finance) |

### Folders
| Folder | Purpose |
|--------|---------|
| `collections/` | Legacy per-industry HTML files (advertising.html, technology.html, etc.) — superseded by unified `kartel-industry.html` |
| `emails/` | Outbound email template HTML files per industry vertical |

## Cross-Link Map
```
kartel-creative-v2.html
  ├── → kartel-build.html          (+ New collection button)
  ├── → kartel-industry.html?industry=X  (5 industry cards in Industry Portfolios panel)
  └── → kartel-view.html?collection=X    (client collection cards)

kartel-build.html
  └── → kartel-creative-v2.html    (Back to Portfolio, Cancel, post-save redirect)

kartel-industry.html
  └── self-contained              (directory ↔ industry views within same page)

kartel-view.html
  └── self-contained              (client-facing, no internal back-links)
```

## URL Parameter System

### kartel-industry.html
- `?industry=advertising|technology|entertainment|consumer|finance` — selects vertical
- `&company=CompanyName` — personalizes page with "KARTEL x COMPANY" co-branding
- `&accent=hexcolor` — optional color override
- No params → shows directory of all 5 verticals
- Password gate per vertical: `KARTEL-CREATIVE2026` (master: `KartelStudios2026!`)

### kartel-view.html
- `?collection=slug` — loads specific client collection from localStorage

## Industry Verticals (5)
| Vertical | Accent Color | Companies | Videos | Password |
|----------|-------------|-----------|--------|----------|
| Advertising & Creative Services | #E85D75 | 274 (27%) | 18 | KARTEL-CREATIVE2026 |
| Technology & SaaS | #4E9FE5 | 223 (22%) | 16 | KARTEL-CREATIVE2026 |
| Entertainment & Media | #C77DFF | 192 (19%) | 21 | KARTEL-CREATIVE2026 |
| Consumer Brands & Retail | #F5A623 | 151 (15%) | 21 | KARTEL-CREATIVE2026 |
| Finance & Professional Services | #5B8C6E | ~97 (10%) | 13 | KARTEL-CREATIVE2026 |

## Key Patterns
- **Collection data lives in `localStorage['kartel_collections']`** — internal platform writes, external viewer reads
- **Video categories match Google Drive `04_MASTERS` folders exactly**: Demos, Demos Real Estate, Commercials, Commercials Spec, Music Videos, Short Stories, Short Film, TV, Competitions, Promo, Workflows, Demo Elements, Website
- **Thumbnail fallback chain**: lh3 CDN → Drive thumbnail API (w640) → Drive thumbnail API (w400) → branded SVG placeholder
- **Collection cards are rendered dynamically** from localStorage via `renderCollectionCards()` — never hardcode them in HTML
- **Industry portfolios use a single unified file** (`kartel-industry.html`) — not separate per-industry files

## AI Enrichment Pipeline
8 MCP connectors power the Collection Builder: HubSpot, Clay, Gmail, Google Calendar, Slack, Google Drive, Web Search, Figma. See `kartel-collection-enrichment-prompt.md` for the portable enrichment workflow and `Kartel_Content_Portal_AI_Integration_Guide.docx` for the full team guide.

## Always Do
- Open the HTML file in browser after making changes (`open "path/to/file.html"`)
- Use H.264 .mp4 for any video files (not .mov — browsers can't play QuickTime)
- Default badge text to "PRIVATE COLLECTION" — never expose internal CRM stages to clients
- Check `sowDeliverables` exists before rendering the deliverables section
- After saving collection data, call `renderCollectionCards()` to refresh the UI
- Use `kartel-industry.html` for all new industry portfolio links (not `collections/` files)

## Never Do
- Don't hardcode collection cards in HTML — they must come from localStorage
- Don't store API keys, secrets, or passwords in code (passwords are prototype-only, will move server-side)
- Don't commit large video files directly (`.mov` is 175MB, exceeds GitHub limit)
- Don't show client type/stage (Prospect, Established, etc.) on external-facing pages
- Don't create separate per-industry HTML files — use the unified `kartel-industry.html` with URL params

## Deeper Context
See memory files at: `~/.claude/projects/-Users-lenoxhuh-Documents-Kartel-Creative-Portfolio/memory/`
