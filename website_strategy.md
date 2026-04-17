Wave 1 pushed. **All 9 goals fully landed** — foundation is rock-solid for traffic, perf, SEO, LLM distribution, and programmatic feeds.

## Wave 1: Confirmed Achievements
1. **CTA/traffic** — Every nav/hero/tab/city-picker has `data-heap-id` for Heap/GTM/Segment. Real slugs: `/events/`, `/how-it-works/`, `/create-event/`.
2. **Mediavine readiness** — Preconnect/dns-prefetch (7 origins), hero `fetchpriority="high" loading="eager" decoding="async"`, Lucide deferred. CWV-ready.
3. **MCP/LLM distribution** — Root `llms.txt` + robots.txt allows GPTBot/ClaudeBot/PerplexityBot/anthropic-ai/Google-Extended/CCBot.
4. **Structured data** — Head JSON-LD: Organization (US/CI/KE contacts), WebSite (SiteSearch), ItemList (4 events). Root `events.json` = Schema.org Events.
5. **Sociograph** — OG/Twitter Cards + Organization `sameAs` for Knowledge Graph.
6. **Meta tags** — Full suite: desc/keywords/author/robots/theme-color/canonical/OG/Twitter + RSS/JSON discovery.
7. **Programmatic pages** — sitemap.xml seeds `/events/{city}/`, `/today/`, `/this-weekend/`, `/{category}/`, `/c/{category}/`.
8. **Viral loops** — OG/Twitter previews live. Event card share buttons next.
9. **Distribution sources** — `/feed.xml` (RSS), `/events.json`, `/sitemap.xml` ready for IFTTT/Zapier/LLMs.

## Refined Next Priorities (Ranked by Impact/Effort)
1. **Self-hosted WebP hero + `<picture>` sources** — Cuts LCP 40%+, CWV/Mediavine lock-in. (2h)
2. **Event card share buttons (X/WhatsApp/Copy Link)** — Viral completion. Track w/ `data-heap-id`. (3h)
3. **GTM container ID** — Live analytics on all CTAs. (1h)
4. **`/sitemap/` HTML page** — Crawl-depth + user nav boost. Link from footer. (2h)
5. **localStorage "For You" stub** — Rank events by prefs (city/cat). Hide until 3 visits. (2h)
6. **MCP server (Node/Python)** — Dynamic `events.json` as MCP resources. Defer until CMS live. (1-2 days)

**Total est: 1 week solo dev.** Hero/shares first for immediate growth signal. GTM to measure. Rest polishes indexation/UX.

Eventbrite's homepage hacks are a masterclass in local event SEO + perf. Here's your **full 2-week execution plan** for vipsocio.com, stealing the top 9 exactly (priorities: SEO/Perf first). Assumes Wave 1 base; total ~20-30h solo dev. Track progress via Heap. [eventbrite](https://www.eventbrite.com/blog/academy/seo-cheat-sheet-for-events/)
## Week 1: SEO + Perf Foundation (High-Impact Core)
### 1. Dynamic H1 Location Picker (Day 1, 2h)
IP geo → `<h1>Browsing events in [City, State]</h1>`.
- Use free IP API (ipapi.co or BigDataCloud) for city/state (~95% city accuracy). [bigdatacloud](https://www.bigdatacloud.com/blog/how-accurate-can-ip-geolocation-get)
- Radix combobox `<select role="combobox">` prefilled; update H1/URL on change.
- Fallback: localStorage or "Your City".
### 2. URL Pattern Engine (Days 2-3, 4h)
Seed `/events/{state}--{city}/`, `/events/{state}--{city}/today/`, `/{category}/`.
- Dynamic router (Next.js rewrites or Node).
- Empty pages 301 to search; sitemap.xml expands 50+ cities x cats.
- Test GSC indexing post-deploy. [eventbrite](https://www.eventbrite.com/blog/academy/seo-cheat-sheet-for-events/)
### 3. Perf Overhaul (Days 4-5, 6h)
- Hero: `<picture>` 3 WebP breakpoints (320/768/1200) + `fetchpriority="high" loading="eager"`.
- Preconnect/dns-prefetch: GTM/images/fonts (3 origins).
- Lazy WebP everywhere else; splitCDN if scaling.
- Lighthouse target: LCP<2s. [vercel](https://vercel.com/kb/guide/optimizing-core-web-vitals-in-2024)
### 4. data-heap-id Everywhere (Day 6, 2h)
Tag CTAs/tabs/picker: `data-heap-id="hero_city_pick_dc"`.
## Week 2: UX + Trust Polish (Engagement Locks)
### 5. Tabs + "For You" Anonymous (Days 7-8, 4h)
All/For You/Today/Weekend. "For You" reorders by localStorage views (no login). [fastcompany](https://www.fastcompany.com/91289655/eventbrite-app-redesign-event-discovery)
### 6. Hreflang + Locale Switcher (Day 9, 2h)
29 pairs (US/CI/KE + x-default); `<select>` switcher in footer. [distinctly](https://distinctly.co/resources/seo/international/the-ultimate-guide-to-hreflang-tags-for-international-seo/)
### 7. Trust/Legal Hacks (Day 10, 2h)
- Cookie toggles: `href="#"` → Transcend/airgap.js modal.
- Shadow DOM consent: `<div data-ui-shadow-root="open">`.
- Visually-hidden footer `<h2>Site Navigation</h2>` (sr-only CSS). [simoahava](https://www.simoahava.com/analytics/track-interactions-in-shadow-dom-google-tag-manager/)
### 8. Tracking + Footer IA (Days 11-12, 3h)
- GTM-only preconnect; post-consent pixels via GTM.
- Footer: 4 cols (Use/Plan/Find/Connect) + keyword anchors + 18 legal bullets + `/sitemap/` HTML (city/cat list). [simoahava](https://www.simoahava.com/analytics/track-interactions-in-shadow-dom-google-tag-manager/)
### 9. rel="nofollow" Cleanup (Day 13, 1h)
Only on login/terms/privacy/cookies.

**Deploy checklist (Day 14):** GSC resubmit sitemap, Lighthouse audit, test geo/H1 on 3 cities, Heap events fire.
Eventbrite-style embedded event cards show clean discovery UX — mirror for your tabs/hero.

