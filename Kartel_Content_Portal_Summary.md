# Kartel Content Portal — Project Summary

**Date:** April 6, 2026
**Status:** Active development — visual prototype complete, architecture designed
**Team:** Lenox (engineering lead), Luke (project owner / spec), Wayan (parallel workstream)

---

## What This Is

The Kartel Content Portal is a branded, interactive platform for sharing Kartel's creative deliverables — primarily video sizzle reels, but expandable to images, pitch decks, and other media — in a polished, access-controlled experience that replaces raw Google Drive links.

Think of it as a private Netflix for Kartel's content: dark-themed, branded, password-protected, and designed to make an impression when shared with clients, partners, and prospects.

## Where We Are Right Now

We have a working visual prototype (`kartel-creative-v2.html`) that renders all **127 videos** across **15 categories** from Kartel's shared Google Drive. It's a single self-contained HTML file that can be opened in any browser right now.

### What the current prototype includes:

- **Full video catalog** — all 127 videos organized into 15 category sections (Commercials, Music Videos, TV — Masked Singer, Demo Elements, Workflows, etc.)
- **Kartel SVG logo** rendered as a reusable vector throughout the interface — login gate, header, footer — with green stroke outlines matching brand guidelines
- **Password-protected access** — login gate with `KartelStudios2026!` as the current password
- **Real-time search** — filters across video names and categories instantly as you type
- **Category filter pills** — clickable category buttons along the top to narrow by content type, with video counts per category
- **Google Drive video playback** — click any card to open a modal with the Drive embed player (iframe to `/file/d/{ID}/preview`)
- **Drive thumbnail system** — thumbnails pulled from Google's CDN with a 3-level fallback chain (lh3 CDN → Drive thumbnail API at 640px → Drive thumbnail at 400px → branded SVG placeholder)
- **Responsive design** — works on desktop, tablet, and mobile with adaptive grid breakpoints

### Design language (current):

- **Background:** Near-black (#050505) with card surfaces at #0e0e0e
- **Accent color:** Deep Kartel green (#008C32) — used for logo strokes, play buttons, hover states, category pills, section indicators, and the header glow line
- **Typography:** Outfit (display/headings — geometric, modern sans-serif) paired with JetBrains Mono (for technical elements like video counts and the password field)
- **Texture:** Subtle film-grain noise overlay across the entire page to eliminate flat digital feel
- **Login gate:** Animated grid pattern background, centered Kartel SVG logo, minimalist password entry with outlined green button
- **Cards:** 16:9 thumbnail with gradient overlay on hover, glass-blur play button with green tint, green accent line that sweeps across the bottom on hover, subtle lift + scale animation
- **Modal:** Full-width Drive embed player with backdrop blur, video title and category displayed above the player
- **Sections:** Stagger-animated on page load with green vertical bar indicators next to category names

---

## Architecture — How It Works Today

The prototype is currently a **single static HTML file** with all video data embedded as a JavaScript array. Each video entry contains a Google Drive file ID, a display name, and a category string. The page renders category sections dynamically, pulls thumbnails from Drive's CDN, and embeds Drive's preview player in a modal on click.

**Content source:** Google Drive shared folder (`1_ElY5vQL1mgaSPsef_uOYQX1xeQFutLV`), accessible via `ultron@kartel.ai`

**Authentication:** Client-side password check (to be moved server-side in production)

**No external dependencies beyond Google Fonts** — Outfit and JetBrains Mono loaded from Google Fonts CDN. Everything else is vanilla HTML/CSS/JS.

---

## Where This Is Going — The Full Vision

The prototype is the public-facing viewer layer of a larger two-layer platform:

### Layer 1: Public Viewer (what exists now, being productionized)
The Netflix-style streaming page gets deployed to **creative.kartel.ai** as a Next.js application on Vercel. Key production upgrades:

- Password moves from hardcoded client-side JS to a **server-side API route** with the password stored as an environment variable
- Video catalog extracted to a **JSON manifest file** (editable without touching code)
- **Framer Motion** for polished page transitions and modal animations
- **Server-side rendering** for fast initial page loads
- **SSL, edge CDN, and auto-deploy** from GitHub via Vercel

### Layer 2: Admin Dashboard (next phase)
An internal management interface accessible only to @kartel.ai team members via Google OAuth:

- **Drive Browser** — real-time mirror of the shared Google Drive folder with thumbnails, search, and file type indicators
- **Collection Manager** — organize videos (and eventually other media) into categories, reorder them, add/remove content, edit metadata — all without code changes
- **Share Link Generator** — create unique private URLs for specific content collections with optional email/passcode access gates (e.g., `creative.kartel.ai/view/abc123` shows only the files you selected for that link)
- **Analytics Dashboard** — per-link view metrics (who opened it, when, how long), per-video play counts, timeline charts, CSV export
- **Slack notifications** — alerts when share links are viewed for the first time

### Beyond Video — Selective Content Portal
The platform is designed to expand beyond video into a general-purpose branded content sharing tool:

- **Image galleries** with lightbox viewing
- **Presentation embeds** (Google Slides, pitch decks)
- **PDF and document viewers**
- **Multiple layout templates** — Showcase (hero video + grid), Gallery (media grid), Reel (video playlist), Deck (presentation focus)
- **Private collection links** — each link renders only its assigned content, with no visibility into other collections or the admin layer

---

## Tech Stack (Zero New Subscriptions)

| Layer | Technology | Cost |
|-------|-----------|------|
| Framework | Next.js 14 (App Router) | $0 (open source) |
| Hosting | Vercel (free Hobby tier) | $0 |
| Auth (admin) | NextAuth.js + Google OAuth | $0 |
| Auth (public) | Server-side password check | $0 |
| Database | Google Sheets API | $0 (existing Workspace) |
| Content source | Google Drive API | $0 (existing Workspace) |
| Video player | Drive embed + Video.js (open source) | $0 |
| Styling | Tailwind CSS + Framer Motion | $0 (open source) |
| Analytics | Custom event tracking → Google Sheets | $0 |
| Notifications | Slack webhooks | $0 (existing Slack) |

**Total new monthly cost: $0**

---

## Key Files

- `kartel-creative-v2.html` — Current working prototype (open in any browser)
- `Kartel_Content_Portal_Unified_Build_Plan.docx` — Full build plan with phased timeline, database schema, and architecture details
- Video catalog: 127 entries with Drive file IDs embedded in the prototype source (ready to export as JSON)

---

## How to Integrate / Collaborate

The public viewer is a standalone page that pulls content from Google Drive via file IDs. The architecture is designed so that:

1. **The video catalog is data-driven** — adding or reorganizing content means editing a JSON file (or, in V2, using the admin UI). No frontend code changes needed.
2. **The Drive integration is read-only** — the platform mirrors whatever is in the shared Drive folder. Upload a video to Drive and it becomes available to add to the portal.
3. **Share links are isolated** — each generated link only sees its own content. The system supports multiple independent collections served from the same codebase.
4. **The design system is tokenized** — colors, fonts, and spacing are defined as CSS variables / Tailwind tokens. Brand changes propagate everywhere instantly.

If you're building anything that touches the shared Drive content or the branded viewing experience, this platform is designed to be the connective layer. Happy to walk through the prototype live or share the source for review.
