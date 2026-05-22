# ADR-003: Typography pairing lock

**Status:** Accepted
**Date:** 2026-05-21
**Owner:** Vista (per Potter dispatch · Wave 3 Batch 1 · STORY-AURORA-014)
**Story:** TODO-238 · STORY-AURORA-014

## Context

Hero V6 ships with Inter as the active typeface across the page (declared in the `body` font-family stack at line 188 of `index.html`). The build brief calls for a locked editorial-grade pairing: a serif display face for headlines (Fraunces 700), a humanist sans for body (Inter variable), and a monospace face for code and data (JetBrains Mono).

Performance gates (Lighthouse Perf ≥95, FCP <1.2s, no FOIT) require self-hosted fonts with `font-display: swap`, Latin subset, and preload of the critical weight (Fraunces 700).

## Decision

Lock the following typography stack for `unrulylabslp.com`:

```
Headline · Fraunces 700        (variable serif · editorial display)
Body     · Inter variable      (humanist sans · 400-700)
Mono     · JetBrains Mono 400  (code / data / receipts)
```

### Loading strategy

- **Self-host preferred** over Google Fonts CDN once a font-hosting plan exists. Until then, use Google Fonts CSS API with `display=swap` and `subset=latin`.
- **Preload** the Fraunces 700 file in `<head>` to eliminate FOIT on the hero `<h1>`.
- **font-display: swap** on all `@font-face` declarations so text renders in a fallback face immediately and swaps in the web-font when ready (no invisible-text-flash).
- **Subset to Latin** to keep total font payload under 80KB across all three families.

### Fallback stacks

```css
body {
  font-family: 'Inter', ui-sans-serif, system-ui, -apple-system,
               "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
}

h1, h2, h3, h4, .hero-h1, .section-h, .cta-h2 {
  font-family: 'Fraunces', Georgia, 'Times New Roman', Times, serif;
}

code, pre, .mono {
  font-family: 'JetBrains Mono', ui-monospace, SFMono-Regular,
               Menlo, Monaco, Consolas, monospace;
}
```

The fallback stacks are chosen so that the page reads at near-identical optical weight before the web-font swaps in (Georgia ≈ Fraunces 700; system-ui ≈ Inter; SFMono-Regular ≈ JetBrains Mono).

### Variable axis usage (Inter)

Use Inter as a variable font with `font-weight` declared at 400 / 500 / 600 / 700 / 800. The variable file delivers all weights in a single payload.

## Rationale

- **Voice.** Fraunces is editorial-warmth-meets-precision; pairs with the Stripe-class hero copy without feeling tech-bro-cold. Operator buyers respond to "company that has a point of view," not generic geometric sans.
- **Performance.** Inter variable (one file, all weights) + Fraunces 700 (preload, one file, one weight) + JetBrains Mono 400 (one file) = three font fetches total. Latin subset keeps the budget under 80KB. Preload prevents FCP penalty on the hero headline.
- **No FOIT.** `font-display: swap` is a hard requirement. Better to flash the fallback for 150ms than to render an invisible H1 for 800ms.
- **Brand-mark coherence.** Candidate B brand-mark uses Inter 700 for the wordmark — body and brand-mark share a typeface so the visual signature is consistent.
- **Code/data niche.** JetBrains Mono is the canonical pick for technical receipts (commit SHAs, timestamps, code snippets). Reserved for code/pre and a `.mono` utility class; NOT used for body or display copy.

## Consequences

**Positive:**
- Headlines render in Fraunces 700 — distinctive, editorial, peer-firm-grade.
- Body in Inter variable matches operator buyer's expectation (Stripe, Linear, Vercel, Modal all use Inter or near-clone).
- Performance budget achievable; FCP <1.2s with preload.

**Negative:**
- Three font families = three fetch entries on the network panel, even when self-hosted.
- Variable-font browser support is excellent today but legacy IE/old-mobile fallbacks drop to system-ui.
- Self-hosting adds an asset-pipeline concern (font files in repo OR CDN).

**Mitigations:**
- Subset to Latin and rely on Google Fonts CSS API `display=swap` until self-host pipeline is wired.
- Use `<link rel="preload" as="font" type="font/woff2" crossorigin>` for Fraunces 700 to eliminate FCP penalty.
- Accept the fallback render for the 100-150ms before Fraunces loads; the system serif (Georgia) is a clean enough optical match.

## Implementation (this commit)

`index.html` is updated to:

1. Add `<link rel="preconnect" href="https://fonts.googleapis.com">` + `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>` in `<head>`.
2. Add `<link rel="preload" as="style" href="<google-fonts-css-url>">` for the three families with `display=swap` query and `subset=latin`.
3. Add `<link rel="stylesheet" href="<google-fonts-css-url>">` with the same query for non-blocking style application.
4. Update the body font-family stack to keep Inter first (no change).
5. Add an `h1, h2, h3, h4, .hero-h1, .section-h, .cta-h2 { font-family: 'Fraunces', Georgia, serif; }` rule below the existing `h1, h2, h3, h4` letter-spacing rule.
6. Add a `.mono, code, pre { font-family: 'JetBrains Mono', ui-monospace, ... }` rule.

The defensive force-reveal fallback (line 502 noscript + line 796 JS) is untouched.

## Triggers for re-evaluation

- Self-host pipeline lands (Atlas-side · pending) → migrate from Google Fonts CDN to self-hosted woff2.
- Lighthouse Perf drops below 95 due to font-loading cost → re-evaluate preload strategy or subset further.
- Operator-direct request to change typeface (Ryan or Darsan).

## Related decisions

- **ADR-001:** Single-page IA.
- **ADR-002:** Color palette lock.
- **STORY-AURORA-023:** Brand-mark Candidate B (uses Inter 700 for wordmark).
