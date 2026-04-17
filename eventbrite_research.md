## Reverse-engineering hacks from Eventbrite's homepage

### SEO
- **Dynamic H1 = location picker**. Their H1 is literally `Browsing events in [City]` with city injected from IP geo. One H1 template localized per visitor — unlimited long-tail.
- **URL pattern is the real SEO engine**: `/d/{state}--{city}/events/`, `/ttd/{state}--{city}/`, `/b/{state}--{city}/{category}/{subcategory}/`, plus date slices like `/events--today/`. Every combo is a landing page.
- **29 hreflang pairs** including `x-default` — one canonical source of locale metadata.
- **`rel="nofollow"` only on signin/terms/privacy/cookies** — they preserve PageRank to discovery pages, deny it to utility pages.

### Perf
- **`preconnect` + `dns-prefetch` paired** for 5 origins (Safari ignores preconnect — belt & suspenders).
- **Hero uses `<picture>` with 3 webp breakpoints + `fetchpriority="high"` + `loading="eager"`**. Everything else is lazy webp.
- **Four image CDNs split by purpose** (chrome icons / user uploads / marketing / city tiles) — different cache lifetimes per bucket.
- **Server-rendered skeleton shell, hydrate feed client-side** — looks full instantly, real data streams in.

### Conversion UX
- Location picker is a **Radix combobox with role="combobox"**, prefilled via IP geo.
- Tabs: **All / For You / Today / This Weekend** — "For You" exists even for anonymous users (pure engagement hook).
- **`data-heap-id` on every clickable** (e.g. `homepage_banner_carousel_click_music`) — declarative analytics, zero inline handlers.

### Trust & legal
- **[HACK] Cookie toggles are empty `href="#"` anchors** — Transcend's `airgap.js` hijacks them to open the consent modal. No dedicated page needed. CCPA compliant.
- Consent banner lives in **shadow DOM** (`data-ui-shadow-root="open"`) — adblockers can't hide it.
- **Visually-hidden `h2 "Site Navigation"`** inside the footer for screen readers + SEO.

### Tracking
- Only **GTM is preconnected**. Everything else (Meta, TikTok, Segment pixels) loads through GTM post-consent — keeps initial HTML small and gives marketing A/B control without code deploys.
- `data-testid` doubles for Cypress tests AND product analytics.

### Footer IA
- **4 columns, each deliberate**: Use (product), Plan (B2B SEO), Find (B2C city+category SEO), Connect (support+social).
- **"Plan Events" column is pure keyword-stuffed long-tail** — "Halloween Party Planning", "QR Codes for Event Check-In". Not real nav, it's SEO anchors.
- **Legal row has 18 items separated by bullets** — dense link graph for crawlers.
- **Dedicated HTML sitemap at `/sitemap/`** (not just XML) — funnels crawl budget into the tail of city×category combos.

### What we should steal for VIP Socio
1. Make our **"Browsing events in [city]"** the actual `<h1>` with IP geo fallback.
2. Build the **URL pattern** `/events/{state}--{city}/{category}/` now — even if pages are empty, the structure lets SEO grow.
3. Add **`data-heap-id`** attributes to every CTA on day one — free analytics later.
4. **Preconnect + preload** the 2-3 origins we actually hit (our image CDN, fonts, GTM).
5. **`<picture>` + webp + fetchpriority high** on the hero image.
6. **Shadow-DOM consent banner** if we do EU/CA traffic — harder to adblock.
7. Add a **visually-hidden `h2 Site Navigation`** in the footer.
8. Add `<select>` locale switcher matching hreflang — single i18n source.
9. **"For You" tab for anonymous users** — engagement bait.

Full plan explained: Eventbrite's homepage hacks are a masterclass in local event SEO + perf. Here's your **full 2-week execution plan** for vipsocio.com, stealing the top 9 exactly (priorities: SEO/Perf first). Assumes Wave 1 base; total ~20-30h solo dev. Track progress via Heap. [eventbrite](https://www.eventbrite.com/blog/academy/seo-cheat-sheet-for-events/)
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

This flips vipsocio into Eventbrite-tier: unlimited long-tail SEO + instant-feel perf. Start with #1-3? [eventbrite](https://www.eventbrite.com/blog/academy/seo-cheat-sheet-for-events/)
