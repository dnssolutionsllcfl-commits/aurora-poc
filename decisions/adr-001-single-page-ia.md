# ADR-001: Single-page IA with deep-link section IDs

**Status:** Accepted
**Date:** 2026-05-21
**Owner:** Vista (per Potter dispatch · Wave 3 Batch 1 · STORY-AURORA-009)
**Story:** TODO-233 · STORY-AURORA-009

## Context

The Unruly Labs LP marketing site at `unrulylabslp.com` must answer the question "what does Unruly Labs LP do, and is it credible" inside one scroll without forcing a multi-page navigation pattern. Operator-direct from Ryan (and joint Atlas+Potter recommendation) is to match the scroll-narrative pattern used by peer firms (Frog, IDEO, Stripe, Linear, Modal, Plaid, Vercel) for v1 of the site.

## Decision

v1 of `unrulylabslp.com` ships as a single HTML page (`index.html`) with deep-link anchors for top-level sections. URLs of the form `/#portfolio`, `/#operators`, `/#contact` resolve to scroll positions on the same document, not separate pages.

Specifically, the nav exposes the following deep-link anchors:

- `#main` — top of page (hero) · also the skip-link target
- `#portfolio` — services / lines-of-business block (a.k.a. "What we run")
- `#operators` — principals + agent fleet block
- `#contact` — CTA band + email entry-point

Future expansion (e.g. `/work` case-study deep-section, `/process` methodology section) is added as additional `id="..."` anchors on the same page until a structural reason exists to break to a separate route. Per the brief: `/work` is implemented as a deep-link section ID on the single page, not a separate route.

## Rationale

- **Peer-pattern match.** Frog (`frog.co`), IDEO (`ideo.com`), and operating-firm peers like Stripe, Linear, Modal, Plaid, Vercel all use scroll-narrative or one-pager IA for their marketing surface. Aurora's product-market is the same operator-class buyer; matching their browsing expectation reduces friction.
- **Build-velocity.** Single-page IA cuts the v1 surface area from N pages to 1, which is consistent with Aurora's "scope-call-to-shipped-site, same week" SLA proof. A multi-page IA adds routing, link-graph, and per-page hero/footer maintenance overhead that's not justified at current content volume.
- **Performance.** One HTML payload with shared critical CSS inline lets us hit Lighthouse Perf ≥95 / FCP <1.2s without per-route hydration cost. Per ADR-003 (typography), font-loading also pays off once instead of per-route.
- **SEO.** Single-page IA with strong section structure and JSON-LD Organization schema is sufficient for the brand/positioning queries we care about (`unruly labs lp`, `unrulylabslp.com`). Per-line-of-business landing pages are a Phase 2 concern when paid acquisition starts.
- **Accessibility.** Single-page with `#main` skip-link, semantic landmarks (`<header>`, `<main>`, `<section>`, `<footer>`), and `scroll-padding-top` for sticky nav offset is straightforward to make WCAG-AA. Already in place.

## Consequences

**Positive:**
- One HTML file is the build surface. Vista's edits are predictable and surgical.
- All copy decisions live in one document — easier voice consistency review.
- Deploys are atomic (one file changes, the whole site moves together).

**Negative:**
- Adding a new line-of-business adds page weight to a single document.
- Deep-link section IDs are case-sensitive and brittle to refactor; any rename breaks external links.
- SEO per-LOB landing requires future ADR if/when paid acquisition needs per-LOB targeting.

**Mitigations:**
- Keep section IDs stable (`#portfolio`, `#operators`, `#contact`); never rename without a redirect plan.
- Add JSON-LD Organization schema (and per-section schema where relevant) so each section can rank for its own query class even when on one page.
- Re-evaluate this ADR at the trigger point of "page weight exceeds 200KB transferred" OR "paid acquisition starts and per-LOB landing pages become a measurable lift."

## Triggers for re-evaluation

- Total page size exceeds 200KB transferred (gzipped).
- Per-LOB paid-acquisition campaigns become a board-approved spend line.
- Operator headcount or LOB count grows past 5 user-visible lines (currently 4: Forge / Sentinel / Aurora / ARIA + Aurora-consulting meta-tile per STORY-AURORA-004).

## Related decisions

- **ADR-002:** Color palette lock.
- **ADR-003:** Typography pairing lock (Fraunces + Inter + JetBrains Mono).
- **Hero V6 (LIVE commit `a279710`):** locks the headline + sub-head copy that opens the single page.
- **Defensive force-reveal (commit `f7671a4`):** locks the JS+noscript fallback that prevents blank-page outage if GSAP fails to load.
