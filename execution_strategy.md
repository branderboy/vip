# VIP Socio — Homepage & SEO Execution Plan (v2)

Updated after critique. Fixes: doorway-page risk, over-engineered consent, untrained personalization, missing content / monetization / KPI tracks. Viral loops promoted into Week 2.

---

## Reverse-engineering hacks from Eventbrite's homepage

### SEO
- **Dynamic H1 = location picker**. Their H1 is literally `Browsing events in [City]` with city injected from IP geo. One H1 template localized per visitor → unlimited long-tail.
- **URL pattern is the real SEO engine**: `/d/{state}--{city}/events/`, `/ttd/{state}--{city}/`, `/b/{state}--{city}/{category}/{subcategory}/`, plus date slices like `/events--today/`. Every combo is a landing page — but only because every combo has real event content behind it.
- **29 hreflang pairs** including `x-default` — one canonical source of locale metadata. Only valuable if you have real translated content per locale.
- **`rel="nofollow"` only on signin / terms / privacy / cookies** — they preserve PageRank to discovery pages, deny it to utility pages.

### Perf
- **`preconnect` + `dns-prefetch` paired** for 5 origins (Safari ignores preconnect → belt & suspenders).
- **Hero uses `<picture>` with 3 WebP breakpoints + `fetchpriority="high"` + `loading="eager"`**. Everything else is lazy WebP.
- **Four image CDNs split by purpose** (chrome icons / user uploads / marketing / city tiles) — different cache lifetimes per bucket.
- **Server-rendered skeleton shell, hydrate feed client-side** — looks full instantly, real data streams in.

### Conversion UX
- Location picker is a **Radix combobox with `role="combobox"`**, prefilled via IP geo.
- Tabs: **All / For You / Today / This Weekend**. "For You" exists even for anonymous users — pure engagement hook.
- **`data-heap-id` on every clickable** (e.g. `homepage_banner_carousel_click_music`) — declarative analytics, zero inline handlers.

### Trust & legal
- Cookie toggles are empty `href="#"` anchors that a consent-management script hijacks to open its modal. Only valuable once you have a real CMP and ad partners requiring it.
- Consent banner lives in **shadow DOM** — adblockers can't hide it. Same caveat.
- **Visually-hidden `<h2>Site Navigation</h2>`** inside the footer for screen readers + SEO.

### Tracking
- Only **GTM is preconnected**. Everything else (Meta, TikTok, Segment pixels) loads through GTM post-consent — keeps initial HTML small, gives marketing A/B control without code deploys.
- `data-testid` doubles for Cypress tests AND product analytics.

### Footer IA
- **4 columns, each deliberate**: Use (product), Plan (B2B SEO), Find (B2C city+category SEO), Connect (support + social).
- **"Plan Events" column is pure keyword-stuffed long-tail** — "Halloween Party Planning", "QR Codes for Event Check-In". Not real nav, it's SEO anchors.
- **Legal row has 18 items separated by bullets** — dense link graph for crawlers.
- **Dedicated HTML sitemap at `/sitemap/`** (not just XML) — funnels crawl budget into the tail of city × category combos.

---

## North-star: what we measure

Define a single KPI per week before shipping anything. Vanity metrics (pageviews, "Heap events fire") don't count.

| Week | North-star KPI | Target | How to measure |
|---|---|---|---|
| 1 | **Organic LCP** | < 2.0s mobile | Lighthouse + CrUX |
| 2 | **RSVP / share ratio** | ≥ 15% of sessions trigger an event-card interaction | Heap + GTM |
| 3 | **Return-visit rate** | ≥ 25% of visitors return within 7 days | GA4 |
| 4 | **Indexed city × category URLs** | ≥ 80% of submitted URLs indexed | GSC |

Monetization KPIs (quarter one):
- Ticket fee take-rate per paid event: target 5-8%
- Creator subscription conversion: target 2% of logged-in creators
- Ad RPM (once Mediavine / Raptive approved): target $8-15 desktop, $4-7 mobile

---

## Day 0 — Baseline (2h)

**Before we ship anything, measure the current state.**

- Run Lighthouse (mobile + desktop) on the homepage. Record LCP, FID/INP, CLS, TBT.
- Crawl current site with Screaming Frog (free tier): list all pages, titles, H1s, meta descriptions, canonical tags.
- Set up GA4 + GSC properties. Submit current sitemap.
- Export current `data-heap-id` inventory — what's missing.
- Record mobile / desktop traffic split baseline (even if tiny).

**Deliverable:** `baseline.md` with current numbers. Every later task refers back to this.

---

## Week 1 — Foundation (High-impact core)

Order matters. Don't skip ahead.

### 1. Dynamic H1 location picker (Day 1, 2h) ✅ already done
- IP geo → `<h1>Browsing events in [City]</h1>` with dropdown trigger.
- BigDataCloud client API (no key) for city. localStorage cache on accept.
- Fallback: "Your City".

### 2. Perf overhaul (Days 2–3, 6h)
Start here because every other change gets measured against LCP.
- Hero: `<picture>` with 3 WebP breakpoints (320 / 768 / 1200), `fetchpriority="high"`, `loading="eager"`, `decoding="async"`, explicit `width`/`height`.
- `preconnect` + `dns-prefetch` for image CDN, font host, GTM (3 origins max).
- Lazy WebP on every below-the-fold image.
- Defer non-critical JS.
- **Gate:** Lighthouse mobile LCP must drop below Day-0 baseline by ≥ 1s before moving on.

### 3. URL pattern engine — content-backed (Days 4–6, 8h)
**Don't ship empty pages.** Google treats them as doorways.
- Seed content first: import / scrape 100 real events across 10 cities × 4 categories. Use Eventbrite public feeds, Songkick API, or manual seeding.
- Only then open URLs for crawl: `/events/{city-slug}/`, `/events/{city-slug}/today/`, `/events/{city-slug}/{category}/`.
- Static site generator (Astro or Next.js SSG) — render pages at build time, not on request.
- Programmatic sitemap.xml regenerated on build.
- Internal links: every event card links back to `/events/{city}/` and `/events/{city}/{category}/`.

### 4. `data-heap-id` inventory pass (Day 7, 2h) ✅ partial
Every CTA, nav link, tab, and card action gets a `data-heap-id`. Naming convention: `{section}_{element}_{action}` (e.g. `hero_city-picker_change`, `card_share_copy`).

### 5. Structured data pass (Day 7, 2h) ✅ already started
- `Organization` + `WebSite` + `SearchAction` on homepage ✅
- `Event` JSON-LD on every event card page (Google Rich Results for events).
- `BreadcrumbList` on city / category pages.

---

## Week 2 — Engagement + Viral Loops

Personalization and share loops, now that we have content and perf to support them.

### 6. Viral share loops (Days 8–9, 6h) **← moved in from separate doc**
From Wave-1 demo:
- **Share + copy-link with toast** on every card. Works. Keep.
- **"More like this" drawer** after any click / share — same city, same category, 3 cards. No dead ends.
- **Trending tonight strip** with live-ticking watchers + tickets sold. Keep counter capped + seeded from real data when available, not pure randomness.
- **Save without account** via localStorage — keep.
- **Urgency swap** — Happening Tonight / Tomorrow / This Week / You missed this. Keep.

### 7. "For You" — content-based (Days 10–11, 4h)
Not behavioral on Day 1 (no data yet).
- Version 1: reorder by **last-viewed city + last-viewed category** stored in localStorage.
- Version 2 (once we have 4 weeks of GA4 data): behavioral clustering server-side, fall back to content-based if user is new.

### 8. Tabs + feed filtering (Day 12, 3h) ✅ done
All / For You / Today / This Weekend. Already wired; add URL param persistence (`?tab=today`) so tabs are shareable / crawlable as their own pages.

### 9. Footer IA + HTML sitemap page (Day 13, 3h) ✅ mostly done
- 4 columns (Use / Plan / Find / Connect) ✅
- 18-item legal bullet row ✅
- Build `/sitemap/` as a real HTML page listing every city × category URL, linked from footer. This is the crawl-budget booster.

### 10. Locale + hreflang — scoped (Day 14, 1h)
**Not 29 pairs.** Only locales with real content:
- `en-us` (default)
- `en-ke` (Kenya)
- `fr-ci` (Côte d'Ivoire)
- `x-default` → en-us
Footer `<select>` switcher matches the 3-4 pairs. Expand later when content is actually translated.

---

## Week 3 — Trust, Monetization Plumbing, Content

### 11. Consent — right-sized (Day 15, 2h)
- If no EU/UK traffic yet: simple cookie banner (cookieyes or a 30-line handcoded one). No Transcend, no shadow DOM.
- If EU/UK > 5% of traffic or ad partner demands it: Cookiebot free tier or Osano.
- `/privacy/`, `/terms/`, `/cookies/` pages live. All `rel="nofollow"`.

### 12. Monetization wiring (Days 16–18, 10h)
- Stripe Connect for ticket checkout. Fee = 5% + $0.50.
- Creator subscription tier scaffold (even if launch is later). `/pricing/` page with Free / Pro / Pro+ comparison.
- Ad slot placeholders in content pages where Mediavine / Raptive will inject (after 50k monthly sessions threshold).

### 13. Content ingestion pipeline (Days 19–21, 12h)
The real unblocker. SEO without content = empty URLs.
- Option A: Eventbrite + Ticketmaster API pulls (allowed for aggregation, attribute back).
- Option B: scraping public calendars (Facebook Events public, Songkick, Resident Advisor) — check ToS per source.
- Option C: Creator upload form + verification flow.
- Target: **50 new events / week minimum** across existing city inventory.

### 14. Tracking layer (Day 22, 3h)
- GTM container live, GA4 via GTM, Meta / TikTok pixels fire post-consent only.
- Heap if budget allows — `data-heap-id` is ready.
- `data-testid` on every structural node for Playwright e2e.

### 15. `rel="nofollow"` cleanup (Day 23, 1h)
Only: `/login/`, `/signup/`, `/terms/`, `/privacy/`, `/cookies/`, `/accessibility/`, checkout pages. Every discovery link is fully followable.

---

## Deploy checklist (Day 24)

- [ ] Lighthouse mobile: LCP < 2.0s, INP < 200ms, CLS < 0.1
- [ ] GSC: resubmit sitemap.xml + /sitemap/ HTML page
- [ ] Geo test 5 cities (Atlanta, NY, LA, Miami, Washington) — H1 updates correctly
- [ ] Heap / GA4: 10 key events fire on 3 browsers
- [ ] Mobile share sheet works for WhatsApp / X / Copy
- [ ] Saved events persist after refresh
- [ ] Structured data: Google Rich Results Test passes Organization + Event + BreadcrumbList
- [ ] `robots.txt` + `sitemap.xml` + `llms.txt` reachable from root
- [ ] All city landing pages return 200 with ≥ 5 events rendered
- [ ] Monetization: test ticket purchase end-to-end with Stripe test mode

---

## What gets dropped from v1

- **29 hreflang pairs** → 4 pairs, grow with content.
- **Transcend + Shadow DOM consent** → simple banner until we actually need CMP.
- **Behavioral "For You" on Day 8** → content-based first, behavioral after 4 weeks of data.
- **Empty URL pattern pages** → content-seeded pages only.

## What's added vs v1

- **Day 0 baseline** (can't improve what you don't measure).
- **North-star KPI per week** (vanity metrics out).
- **Week 3: content ingestion pipeline** (the real SEO unblocker).
- **Monetization wiring** (Stripe Connect, pricing page, ad slot placeholders).
- **Viral loops promoted** into Week 2 as a priority track.
- **Deploy checklist** expanded with Rich Results Test + mobile share verification.

---

## Total scope

~60h solo dev over 24 days, across:
- Week 1: 20h (perf + URL engine + structured data)
- Week 2: 17h (loops + personalization + footer)
- Week 3: 28h (consent + monetization + content ingestion)

Content ingestion (task 13) is the single biggest lever. If resources are constrained, do 1 → 2 → 3 → 13 → 6 and skip the rest until traffic justifies it.
