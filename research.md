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
