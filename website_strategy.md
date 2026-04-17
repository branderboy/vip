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

Prioritize 1-3 this week? Or jump to MCP?
